# 结算记录同步接口

### 1 通知地址

合作方提给一个通知地址并实现该接口用于接收结算记录同步通知
- 该接口是回调由对接方实现平台主动调用该接口
- 若通知未能返回`1001`（处理不成功），则平台会重复通知多次

### 2 请求方式

	HTTP POST FORM 表单提交

### 3 请求参数
| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| app_id | string | Y | 平台分配的接入应用ID |
| sign | string | Y | 请求数据签名 |
| merchant | string | Y | 商户号 |
| serial | string | Y | 结算流水，全局唯一 |
| subject | string | Y | 结算主题 |
| create_time | long | Y | 结算记录创建时间, 单位ms |
| start_time | long | Y | 结算记录开始时间, 单位ms |
| end_time | long | Y | 结算记录结束时间, 单位ms |
| trade_count | long | Y | 交易笔数 |
| total_value | long | Y | 总交易金额(分) |
| settle_value | long | Y | 最终结算金额(分) |
| service_value | int |Y |手续费(分)|
| status | short |Y |结算状态: 1 已清算; 0 未清算|
|type |short| Y | 结算类型: 1正常, 2补款, -1扣款, -2退款|
|transfer_status |short| Y | 划账状态: 0, 未划账; 1, 已划账；-1, 划账异常; |
|trade_list | list(Payment) | N |关联支付记录, 仅在未划账会携带该字段, 后续同步划账状态不在携带该字段!!! |

**支付记录**

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| pay_serial | String | Y | 平台支付流水 |
| pay_order | String | Y | 第三方支付流水 |
| value | int | Y | 支付金额(分) |
| fee | int | Y | 手续费(分) |
| refund_value | int | Y | 退款金额(分)，大于0表示有退款 |


### 4 响应参数

| 字段 | 类型 | 必须 | 说明｜
| --- | --- | --- | ----- |
| code | string | Y | 业务处理状态码<br>1001:通知成功<br/>其它状态码:读取message |
| message | string | N | 业务处理状态说明 |
| hint | string | N | 提示说明 |


示例: 
```json
{
	"code": "1001",
	"message": "通知成功",
	"hint": ""
}
```
