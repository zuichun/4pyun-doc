# 续费会员同步

### 1 接口说明
提供地址给平台提前配置，合作方实现该接口用于接受会员续费交易通知, 若通知未能返回`1001`不成功则会重复通知多次。

    POST FORM 表单提交


### 2 请求参数
| 字段 | 类型   | 必须 | 说明 |
| --- | --- | --- | --- |
| app_id      |  string |  Y   | 平台分配的接入应用ID |
| sign | string | Y | 请求数据签名   |
| mobile      | string | N    | 会员手机号          |
| effect_time     | date | Y    | 当前生效时间, 格式`yyyyMMdd` |
| expire_time     | date | Y    | 当前到期时间, 格式`yyyyMMdd` |
| member_type      | string | Y    | 会员类型标识          |
| member_type_name      | string | Y    | 会员类型名称    |
| member_type_model      | string | Y    | 会员套餐标识    |
| version      | string | Y    | 会员权益版本号    |
| type      | int | Y    | 购买类型: 1-新购、2-续费    |
| quantity      | int | Y    | 购买月份数    |
| quantity      | int | Y    | 购买月份数    |
| pay_order     | string | Y    | 支付订单号     |
| channel       | string | Y    | 支付渠道       |
| pay_serial    | string | Y    | 平台支付流水   |
| pay_status  | short  | Y    | 支付状态: 1-支付完成 |
| pay_time    | date   | Y    | 支付时间, 格式`yyyyMMddHHmmss` |
| value         | int | Y    | 支付金额，单位：分        |
| pay_value         | int | Y    | 实付金额，单位：分    |
| refund_value         | int | N    | 已退款金额，单位：分    |
| referral_code         | string | N    | 邀请码    |

### 2 响应参数

| 字段    | 类型   | 必须 | 说明|
| ------- | ------ | ---- | ---------------------------------------------------------- |
| code    | string | Y    | 业务处理状态码<br>1001:通知成功<br/>其它状态码:读取message |
| message | string | N    | 业务处理状态说明          |
| hint    | string | N    | 提示说明       |


### 3 请求返回结果示例

成功返回
```
{
	"code": "1001",
	"message": "OK"
}
```
