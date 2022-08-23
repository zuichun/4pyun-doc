# MySQL索引

## <a name="1"></a>1. 什么是索引？

**索引是一种通过避免查询时全表扫描实现快速得到查询结果而建立的数据结构**; 以下这个例子很好的说明了索引的一种实现以及它如何提升我们的查询效率。

> 假设数据库中一个表有10\^6条记录，DBMS的页面大小为4K，并存储100条记录。
>
> 如果没有索引，查询将对整个表进行扫描，最坏的情况下，如果所有数据页都不在内存，需要读取10\^4个页面，如果这10\^4个页面在磁盘上随机分布，需要进行10\^4次I/O，假设磁盘每次I/O时间为10ms(忽略数据传输时间)，则总共需要100s(但实际上要好很多很多)。
>
> 如果对之建立B-Tree索引，则只需要进行log100(10\^6)=3次页面读取，最坏情况下耗时30ms。^[1]

这就是索引带来的效果，很多时候，当你的应用程序进行SQL查询速度很慢时，应该想想是否可以建索引。

## <a name="2"></a>2. 索引类型

MySQL中索引分为:

- `PRIMARY KEY`直接通过设置主键也就建立了索引;
- `UNIQUE`一般主要用于保证数据的唯一性;
- `INDEX`普通索引也是最常使用的索引类型;
- `FULLTEXT`主要可用于列类型为(`CHAR`,`VARCHAR`,`TEXT`)的快速匹配查询^[3]。

````
注: FULLTEXT索引仅支持MyISAM引擎, InnoDB引擎需要版本>=MySQL5.6才支持!
````
在`WHERE`语句使用`=`,`>`,`<`,`BETWEEN`,`IN`都可使用索引快速查找到特定的某条或某些记录。


## <a name="3"></a>3. 多列索引

MySQL中也可根据多个列创建索引; 例如你可以有基于三个列创建一个多列索引索引INDEX`(col1, col2, col3)`, 那么可用的索引有`INDEX(col1)`, `INDEX(col1, col2)`以及`INDEX(col1, col2, col3)`。^[4]

以下查询是会使用索引:

````
SELECT * FROM tbl_name WHERE col1=val1;
SELECT * FROM tbl_name WHERE col1=val1 AND col2=val2;
SELECT * FROM tbl_name WHERE col1=val1 AND col2=val2 AND col3=val3;
````
以下查询则不会使用索引

````
SELECT * FROM tbl_name WHERE col2=val2;
SELECT * FROM tbl_name WHERE col2=val2 AND col3=val3;
````

## <a name="4"></a>4. 使用最优索引

当表存在多个索引, MySQL只会选择`MYSQL QUERY Optimizer`认为最有的一个索引进行查询而放弃其他索引, 绝大多数情况下MySQL都可以自动选择到最优的索引。

**索引选择策略:^([2])**

- `WHRER`之后的字段会被纳入索引候选名单;
- 存在多个索引时, 优先使用盖索引键值最短的索引;
- 存在多列索引时, 会使用任何最左前缀匹配的索引用于查询;
- 根据`INDEX HINT`优先/强制使用某些索引^([5])。


## <a name="o"></a>总结

1. 以下情况可考虑是否索引存在问题:

    - IOPS居高不下
    - CPU利用率居高不下
    - SQL执行缓慢

2. 定期观察SQL执行情况, 如果有执行时间过长的SQL, 应该EXPLAIN^([6])看看是否存在全表扫描或者过半记录扫描。
3. 如果`MYSQL QUERY Optimizer`没有选择最优的索引, 则通过`INDEX HINT`主动指定当前SQL使用的索引。

## <a name="x"></a>附录
[1] 理解MySQL - 索引与优化
http://www.cnblogs.com/hustcat/archive/2009/10/28/1591648.html
[2] How MySQL Uses Indexes
https://dev.mysql.com/doc/refman/5.6/en/mysql-indexes.html
[3] InnoDB FULLTEXT Indexes
https://dev.mysql.com/doc/refman/5.6/en/innodb-fulltext-index.html
[4] Multiple-Column Indexes
https://dev.mysql.com/doc/refman/5.6/en/multiple-column-indexes.html
[5] Index Hints
https://dev.mysql.com/doc/refman/5.6/en/index-hints.html
[6] Optimizing Queries with EXPLAIN
https://dev.mysql.com/doc/refman/5.6/en/using-explain.html
[7] MySQL 索引选择原则
http://blog.chinaunix.net/uid-26896862-id-3328675.html
