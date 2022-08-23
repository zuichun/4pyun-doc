# 基于Redis实现分布式定时任务

## <a name="1"></a>1. 技术背景

### 1.1. Redis Keyspace Notifications

从`Redis 2.8.0+`开始Redis提供了`Keyspace Notifications`[^1]特性; 这一特性使得客户端可以通过发布/订阅来接收redis影响数据集相关事件, 例如:

* 新建KEY

* 对KEY执行了`LPUSH`操作

* KEY过期

#### 1.1.1 配置

由于该特性会新增CPU消耗, `keyspance events notifications`是默认关闭的, 可通过修改redis.conf或`CONFIG SET` 配置`notify-keyspace-events`来开启,

```textile
K     Keyspace events, published with __keyspace@<db>__ prefix.
E     Keyevent events, published with __keyevent@<db>__ prefix.
g     Generic commands (non-type specific) like DEL, EXPIRE, RENAME, ...
$     String commands
l     List commands
s     Set commands
h     Hash commands
z     Sorted set commands
x     Expired events (events generated every time a key expires)
e     Evicted events (events generated when a key is evicted for maxmemory)
A     Alias for g$lshzxe, so that the "AKE" string means all the events.
```

配置中至少需要出现`K`/`E`, 否则将不会接收到任何事件, 如果配置为`KEA`则会接收到任何可能的事件。

```
#  specify at least one of K or E, no events will be delivered.
notify-keyspace-events "KEA"
```

*注意: Redis的发布/订阅阅后即焚是不支持持久化的, 故如果客户端断开重连则在这期间的消息将丢失!*

#### 1.1.2 测试

订阅事件

```
s1.vm.net:6379> PSUBSCRIBE __keyevent@*__:expired
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "__keyevent@*__:expired"
3) (integer) 1
```

过期一个KEY

```shell
SET foo val EX 10
```

收到通知

```shell
1) "pmessage"
2) "__keyevent@*__:expired"
3) "__keyevent@0__:expired"
4) "a"
```

#### 1.1.3 RedisKeyExpiredEvent

网上实际有很多其他方案, 在`spring-data-redis`中已提供了对上面特性的实现只是很少有人介绍到, 我推荐使用以下方案, 则每当有KEY失效则以下listener会收到消息:

```java
public @Bean ApplicationListener<RedisKeyExpiredEvent> redisKeyExpiredEventListener() {
        return event -> {
            System.out.println(String.format("A Received expire event for key=%s with value %s.", new String(event.getSource()), event.getValue()));
        }
}
```

实现原理是在`org.springframework.data.redis.listener.KeyExpirationEventMessageListener`中订阅事件`__keyevent@*__:expired`如下:

```java

public class KeyExpirationEventMessageListener extends KeyspaceEventMessageListener implements
        ApplicationEventPublisherAware {

  private static final Topic KEYEVENT_EXPIRED_TOPIC = new PatternTopic("__keyevent@*__:expired");

    @Override
    protected void doRegister(RedisMessageListenerContainer listenerContainer) {
        listenerContainer.addMessageListener(this, KEYEVENT_EXPIRED_TOPIC);
    }
  ...
}
```

### 1.2 Distributed Locks

有多种方式去实现分布式锁, 关于使用Redis做分布式锁我推荐大家可以看看附录[^2]官方的文章, 里面详细介绍了官方推荐的正确的实现方式。

#### 1.2.1 RedisLockRegistry

在`Spring Integration`[^3]中从4.0开始就提供了一种基于redis的分布式锁实现`RedisLockRegistry`, 可用过用`obtain`方法直接获取到`java.util.concurrent.locks.Lock`也很简单:

```java
// 1. 创建对象
public @Bean RedisLockRegistry redisLockRegistry(RedisConnectionFactory connectionFactory) {         return new RedisLockRegistry(connectionFactory, "Foo-API");
}

@Autowired
private RedisLockRegistry redisLockRegistry;

// 并发方法
public void foo() {
    java.util.concurrent.locks.Lock lock = null;
    try {
        lock = redisLockRegistry.obtain(DistributedLockService.createLockKey(trigger));
        if (!lock.tryLock()) {
            // 未获取到锁
            return;
        }
        // 已成功获取到分布式锁
    } finally {
         // Unlock safely
         if (lock != null) try { lock.unlock(); } catch (Exception e) { /* NOTHING */ }
    }
}
```

#### 1.2.3 java.util.concurrent.locks.Lock

根据实际的需求选择使用`tryLock`/`lock`来实现我们的具体场景, java中对该对象定义如下:

```java
public interface Lock {
    /**
     * Acquires the lock.
     *
     * <p>If the lock is not available then the current thread becomes
     * disabled for thread scheduling purposes and lies dormant until the
     * lock has been acquired.
     */
    void lock();

    /**
     * Acquires the lock unless the current thread is
     * {@linkplain Thread#interrupt interrupted}.
     *
     * <p>Acquires the lock if it is available and returns immediately.
     *
     * <p>If the lock is not available then the current thread becomes
     * disabled for thread scheduling purposes and lies dormant until
     * one of two things happens:
     *
     * <ul>
     * <li>The lock is acquired by the current thread; or
     * <li>Some other thread {@linkplain Thread#interrupt interrupts} the
     * current thread, and interruption of lock acquisition is supported.
     * </ul>
     *
     * <p>If the current thread:
     * <ul>
     * <li>has its interrupted status set on entry to this method; or
     * <li>is {@linkplain Thread#interrupt interrupted} while acquiring the
     * lock, and interruption of lock acquisition is supported,
     * </ul>
     * then {@link InterruptedException} is thrown and the current thread's
     * interrupted status is cleared.
     *
     * @throws InterruptedException if the current thread is
     *         interrupted while acquiring the lock (and interruption
     *         of lock acquisition is supported)
     */
    void lockInterruptibly() throws InterruptedException;

    /**
     * Acquires the lock only if it is free at the time of invocation.
     *
     * <p>Acquires the lock if it is available and returns immediately
     * with the value {@code true}.
     * If the lock is not available then this method will return
     * immediately with the value {@code false}.
     *
     * <p>A typical usage idiom for this method would be:
     *  <pre> {@code
     * Lock lock = ...;
     * if (lock.tryLock()) {
     *   try {
     *     // manipulate protected state
     *   } finally {
     *     lock.unlock();
     *   }
     * } else {
     *   // perform alternative actions
     * }}</pre>
     *
     * This usage ensures that the lock is unlocked if it was acquired, and
     * doesn't try to unlock if the lock was not acquired.
     *
     * @return {@code true} if the lock was acquired and
     *         {@code false} otherwise
     */
    boolean tryLock();

    /**
     * Acquires the lock if it is free within the given waiting time and the
     * current thread has not been {@linkplain Thread#interrupt interrupted}.
     *
     * <p>If the lock is available this method returns immediately
     * with the value {@code true}.
     * If the lock is not available then
     * the current thread becomes disabled for thread scheduling
     * purposes and lies dormant until one of three things happens:
     * <ul>
     * <li>The lock is acquired by the current thread; or
     * <li>Some other thread {@linkplain Thread#interrupt interrupts} the
     * current thread, and interruption of lock acquisition is supported; or
     * <li>The specified waiting time elapses
     * </ul>
     *
     * <p>If the lock is acquired then the value {@code true} is returned.
     *
     * <p>If the current thread:
     * <ul>
     * <li>has its interrupted status set on entry to this method; or
     * <li>is {@linkplain Thread#interrupt interrupted} while acquiring
     * the lock, and interruption of lock acquisition is supported,
     * </ul>
     * then {@link InterruptedException} is thrown and the current thread's
     * interrupted status is cleared.
     *
     * <p>If the specified waiting time elapses then the value {@code false}
     * is returned.
     * If the time is
     * less than or equal to zero, the method will not wait at all.
     *
     * @param time the maximum time to wait for the lock
     * @param unit the time unit of the {@code time} argument
     * @return {@code true} if the lock was acquired and {@code false}
     *         if the waiting time elapsed before the lock was acquired
     *
     * @throws InterruptedException if the current thread is interrupted
     *         while acquiring the lock (and interruption of lock
     *         acquisition is supported)
     */
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    /**
     * Releases the lock.
     */
    void unlock();

    ...
}
```

## <a name="2"></a>2. 设计思想

![diagram](./media/diagram.png)

### 2.1 任务管理

定义任务管理服务, 用于受理其他服务程序通过RPC/DB/MQ等任务创建指令, 该服务根据任务等元数据(META DATA)判断任务是需要立即执行或是延时执行。

* **立即执行** - 立即把任务交接给`任务执行`立即开始执行。

* **延时执行** - 将任务数据存入`Redis`并设置`TTL = (执行时间 - 当前时间)`。

### 2.2 执行任务

根据不同等任务数据调用不用等任务具体实方法去执行任务, 例如执行一条SQL、执行一个RPC调用等, 执行成功则任务调度完成, 执行不成功则根据任务元数据(META DATA)来控制任务执行情况, 例如可约定以下数据:

```sh
RETRY_INTERVAL = 3000 # 任务失败重试间隔
MAX_RETRIES = 3 # 任务失败最大重试次数
```

当任务执行失败且还满足可执行条件, 则根据配置`RETRY_INTERVAL`将任务数据放入`Redis`并设置`TTL = RETRY_INTERVAL`, 则任务则会在`TTL`之后重新被执行。

根据前面技术背景中提到当`Redis`现有当特性, 以及前面我们根据KEY的TTL来控制任务的执行, 则收到KEY过期事件即代表任务达到执行时间了; 但在分布式环境中, 多个JVM会同时监听到KEY过期, 为了防止任务重复执行, 所以在可执行任务前需要再结合分布式锁获取到锁的JVM方可执行任务, 否则直接忽略该事件, 因为其他JVM已经执行了该任务。

## <a name="3"></a>3. 总结

本文描述的方案主要结合了`Redis`两大特性:

* Keyspace Notifications[^1]

* 基于Redis的分布式锁

来实现来分布式任务调度, 都基于Redis来实现, 较大程度发挥了其自身优势, 相较于`quartz`[^4]更加轻量级。

## <a name="qa"></a>常见问题

* KEY过期没有触发失效事件

检查redis中`notify-keyspace-events`配置情况, 或者直接通过`redis-cli`连接到redis执行`MONITOR`指令观察消息情况。

## <a name="x"></a>附录

[1] Redis Keyspace Notifications

https://redis.io/topics/notifications

[2] Distributed locks with Redis

https://redis.io/topics/distlock

[3] Spring Integration

https://spring.io/projects/spring-integration

[4] Quartz Enterprise Job Scheduler

http://www.quartz-scheduler.org/
