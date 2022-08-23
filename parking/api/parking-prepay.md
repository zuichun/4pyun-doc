# 无感停车状态同步

**协议说明:**

当用户满足无感支付条件时, P云主动向停车场系统发起设置车辆自动支付权限, 停车场可以根据`credits`在车场离场时选择是否触发主动扣费实现无感停车。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| credits | int | Y | 当前用户可用信用额度(分),  credits大于零则开启. |
| parking_serial | string | Y | 停车流水, 标识具体某次停车事件, 需保证该停车场下唯一. |
| plate | string | N | 车牌|
| pay_origin | int | N | 值    含义<br/>0   P云<br/>4   支付宝<br/>8   微信<br/>-    兼容其他未定义 |
| pay_origin_desc | string | N | 支付来源说明, 例如:P云 |
| pay_source | string | N | 付款来源, 例如: 微信/支付宝/银联 |
| locking | int | Y | 锁定车辆, 解锁后可出场:1锁定, 0不锁;|
| deduct_mode | string | N | 扣款模式 |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1002  停车记录不存在<br/>1401  签名错误, 请检查配置.1403     车辆已出场.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

示例:
```bash
POST https://demo.4pyun.com/parking/1.0/gateway, timeout：10000ms
Request Body：
{
  "charset" : "UTF-8",
  "sign" : "5DDE645BDCD7C54D805258B4A40A13D7",
  "park_uuid" : "1239",
  "plate" : "粤A9T58H",
  "service" : "service.parking.vip.query",
  "version" : "1.0"
}
Response Body：
{
  "result_code" : "1001”,
  "message" : "OK",
}
```
