# 同步临停缴费结果

**协议说明:**

当用户完成支付后, P云将主动发起支付结果通知, 通知客户端订单支付结果.

注:
 1. 为保证通知正常处理, 服务端可能发起多次支付结果通知, 客户端需做好去重逻辑.<br/>
 2. 对于已经受到支付结果通知的订单, 应应答通知成功, 已告知服务端不必继续通知.
</font>

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.payment.result` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| parking_serial | string | Y | 停车流水, 原车场返回, 付费入场可能为空!|
| parking_order | string | Y | 停车支付订单号, 原车场返回. |
| gate_id | string | N | 通道编号ID, 付费时可传递触发开闸. |
| plate | string | N | 车牌号码, 当无牌车付费入场时为虚拟车牌. |
| pay_serial | string | Y | P云支付流水, 对账可用. |
| pay_time | string | Y | 支付时间, 格式:`yyyyMMddHHmmss`. |
| value | int | Y | 支付金额(单位分), 不含优惠金额 . |
| free_value | int | N | 优惠金额(单位分) . |
| pay_origin | int | Y | 值 含义<br/>0    P云<br/>4   支付宝<br/>8    微信<br/>-     兼容其他未定义|
| pay_origin_desc | string | Y | 支付到账说明, 例如:P云|
| pay_source | string | N | 付款来源, 例如: 微信/支付宝/银联 |
| autopay_type | short | N | 自动扣费类型: 0非自动扣费, 1先出后扣, 2先扣后出 |

*请求示例*
```json
{
  "charset":"UTF-8",
  "park_uuid":"3bad72c0-6204-4b65-90aa-4323ddd1fb5a",
  "parking_order":"201811301049350025",
  "parking_serial":"8c598afd-2cb8-4cec-8f73-4fd4391d2fc6",
  "pay_origin":"4",
  "pay_origin_desc":"支付宝",
  "pay_source": "支付宝",
  "pay_serial":"20181130105240075500112137",
  "pay_time":"20181130105250",
  "service":"service.parking.payment.result",
  "sign":"E7D7371549C2B4E3AE38C3A028973020",
  "value":"500",
  "version":"1.0"
}
```

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.payment.result` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 状态码<br/>值 含义<br/>1001  接口处理成功.<br/>1401  签名错误, 请检查配置.<br/>1403  订单已撤销.1.车辆出场2.参考7章节<br/>1500  接口内部处理失败. |
| sign | string | Y | 签名|
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| parking_serial | string | N | 付费入场返回实际停车流水! |

*应答示例*
```json
{
  "charset":"UTF-8",
  "message":"订单支付成功",
  "result_code":"1001",
  "service":"service.parking.payment.result",
  "sign":"F97B3D16E739C82C1DED0450775905CF",
  "version":"1.0"
}
```
