# 发放商户优惠券

**协议说明:**

P云通过和停车场系统完成优惠对接, 将停车场优惠券实现纯电子化.
`无论用户移动支付或现金支付, 停车场系统均可读取到本次停车关联的停车优惠并自动抵扣停车费.`

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.discount.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段   | 类型 | 必须 | 说明   |
| --- | --- | -- | ---|
| park_uuid | string | Y | 停车场编号|
| parking_serial | string | N | 停车流水, 标识具体某次停车事件, 需保证该停车场下唯一.      |
| plate | string | N | 忽略停车状态派发时传递        |
| grant_serial | string | Y | 优惠券派发流水.  |
| type | int | Y | 优惠类型: 1金额, 2时长, 3全免, 4不同计价券, 5长期免费券, 6折扣券 |
| value | int | Y | 优惠金额:<br/>当`type=1`时单位为分;<br/>当`type=2`时单位为分钟;<br/>当`type=3`时暂无定义;<br/>当`type=4`时值无效;<br/>当`type=5`时为在派发后单位分钟后重复进出均可免费停车;<br/>当`type=6`时取值范围1-1000, 1表示0.1%, 800表示80%. |
| reason | string | N | 优惠给予原因, 例如:购物满300, 免费停车2小时.        |
| store_name | string | Y | 当前派发优惠的商家名称.       |
| store_code | string | Y | 当前派发优惠的商家唯一标识.     |
| coupon_name | string | Y | 优惠券名称 |
| coupon_code | string | Y | 平台优惠券ID |
| coupon_rule | string | N | 优惠券减免规则ID, 用于配置减免是减免前面还是减免后面一般一个停车场配置相同.       |
| charge_rule | string | N | 不同计价券收费规则ID, 用于配置改优惠券后本次停车使用不同计价规则, 一般每种优惠券不同.          |
| expire_time | string | N | 指定券失效时间, 格式: `yyyyMMddHHmmss` |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.discount.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  优惠成功.<br/>1002  停车信息未找到.<br/>1403  当前车辆已享受其他优惠.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

注:
1. 优惠券的真实使用核销(一般是在参与了支付或者车辆已经出场核销之后不能再被撤销需停车场本地控制).
2. 优惠券实际优惠值在查费用接口返回(比如优惠了2小时最终查费用的时候按实际抵扣情况返回优惠了对应金额)
3. expire_time如果未指定 按停车场实际有效期来(优惠券有效时间停车场系统内部定义)
