# 2.3 开票结果异步通知

### 通知地址
 开票请求接口请求参数中的notify_url
- 描述
  合作方实现该接口用于接受支付结果通知, 若通知未能返回`1001`不成功则会重复通知多次。
### 调用方式

	HTTP POST FORM 表单提交

### 特殊说明
        合作方实现该接口用于接受电子发票开票结果通知, 若通知未能返回`1001`不成功则会重复通知多次。

### 请求参数

| 字段          | 类型   | 必须 | 说明                              |
| ------------- | ------ | ---- | --------------------------------- |
| app_id             | 平台分配的接入应用ID                                         | string |  Y   | op1234567723122                  |
| sign               | 请求数据签名                                                 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant      | string | Y    | 商户号                            |
| obtain_order  | string | Y    | 合作方开票请求订单                |
| obtain_serial | string | Y    | 平台方开票唯一标识                |
| status        | short  | Y    | 开票状态: 2 开票成功、-1 开票失败 |
| status_desc   | string | N    | 状态说明                          |
| invoice_code  | string | N    | 发票代码                          |
| invoice_no    | string | N    | 发票号码                          |
| verify_code   | string | N    | 发票校验码                        |
| invoice_url   | string | N    | 发票链接                          |
| invoice_time  | long   | N    | 交易时间, unix时间戳, 单位ms      |

### 响应参数

| 字段    | 类型   | 必须 | 说明                                                       |
| ------- | ------ | ---- | ---------------------------------------------------------- |
| code    | string | Y    | 业务处理状态码<br>1001:通知成功<br/>其它状态码:读取message |
| message | string | N    | 业务处理状态说明                                           |
| hint    | string | N    | 提示说明                                                   |


### 请求返回结果示例

```
成功返回
{
	"code": "1001",
	"message": "通知成功",
	"hint": ""
}
```
