# 停车场实时车位

**协议说明:**

P云将以固定频率调用该接口获取总停车位和收费标准的获取.

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.realtime` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |

请求示例:

```json
POST https://xxxx/v1/gateway

{
  "charset" : "UTF-8",
  "sign" : "B2B6A0FC5816B5174572172A777E8DD3",
  "park_uuid" : "10315",
  "service" : "service.parking.realtime",
  "version" : "1.0"
}
```

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.realtime` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  获取成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| total | int | Y | 停车场总车位数. |
| parking | int | Y | 当前场中车辆数量. |
| charge_json | string | Y | 收费规则数据JSON格式字符串, 按合作方自行组装, 例如: {"free_time":15,"max":3000,"more":{"price":300,"unit":60},"type":1} |
| charge_depict | string | Y | 收费规则文字描述, 例如: 入场15分钟免费, 3元/小时。 |

应答示例:

```json
{
  "parking" : 3541,
  "service" : "service.parking.realtime",
  "charge_json" : "{\"data\":[{\"price\":5},{\"price\":5},{\"price\":10},{\"price\":10},{\"price\":10},{\"price\":10},{\"price\":10},{\"price\":10}],\"max\":0,\"free_time\":30,\"type\":5,\"addFlag\":false}",
  "result_code" : "1001",
  "message" : "成功",
  "charge_depict" : "入场免费30分钟,一天最收费10元,",
  "total" : 1000000,
  "version" : "1.0"
}
```
