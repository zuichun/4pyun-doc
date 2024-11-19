# 分账交易同步

### 1 通知地址

合作方提给一个通知地址并实现该接口用于接收分账记录同步请求
- 该接口是回调由对接方实现平台主动调用该接口
- 若通知未能返回`1001`（处理不成功），则平台会重复通知多次

### 2 请求方式

	POST JSON

### 3 请求参数
| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| app_id | string | Y | 平台分配的接入应用ID |
| sign | string | Y | 请求数据签名 |
| merchant | object | Y | 交易商户 |
| - code | string | Y | 交易商户编号 |
| - name | string | Y | 交易商户名称 |
| partner | object | Y | 分账商户 |
| - code | string | Y | 分账商户编号 |
| - name | string | Y | 分账商户名称 |
| trade_scene | string | Y | 交易场景标识 |
| trade_no | string | Y | 分账交易单号 |
| sharding_type | short | Y | 分账类型: 0-交易比例, 1-定期扣款 |
| trade_type | short | Y | 分账交易类型: 1-收入, -1-扣款 |
| status | short |Y | 分账记录状态: 1-有效, 0-无效|
| sharding_value | int | Y | 原交易可分账金额, 单位: 分 |
| trade_value | int | Y | 分账金额, 单位: 分 |
| rule_value | int | Y | 规则分账金额/分账比例, 若按交易事项分账该字段无效 |
| trade_time  | date | Y | 交易时间, 格式`yyyyMMddHHmmss` |
| trade_item | list | N | 分账交易事项 |
| - key | string | Y | 交易事项标识 |
| - rate | int | Y | 分账比例, 千分之X |
| - value | int | Y | 分账金额, 单位: 分 |

示例: 
```json
{
  "app_id" : "op00961963581daa7",
  "rule_value" : 0,
  "sharding_type" : 0,
  "timestamp" : 1731973767578,
  "trade_time" : "20241118191701",
  "merchant" : {
    "name" : "P云支付体验-停车场",
    "code" : "62626601"
  },
  "partner" : {
    "name" : "测试渠道商1019MIX-J",
    "code" : "87105025"
  },
  "trade_item" : [
    {
      "key" : "energy_value",
      "value" : -50,
      "rate" : 500
    },
    {
      "key" : "service_value",
      "value" : -13,
      "rate" : 500
    }
  ],
  "trade_no" : "20241118191500066020111624",
  "trade_value" : -63,
  "sharding_value" : 124,
  "trade_type" : -1,
  "sign_type" : "MD5",
  "trade_scene" : "ENERGY",
  "status" : 1,
  "sign" : "E6020591B98425D5D412CCE0B58A1629"
}
```

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