# 获取固定车类型

**协议说明:**
P云发起请求, 查询车场固定车类型

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.charge_type.list` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号|

**说明**
- 返回车场所有有效状态的收费标准

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.charge_type.list` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| row | array | N | 收费类型集合, 对象参考后面收费类型信息. |


*应答示例*

```json
{
  "service" : "service.parking.charge_type.list",
  "row" : "[{\"id\":\"21\",\"name\":\"测试月租A\",\"vip_type\":1,\"desc\":\"测试月租A\",\"price\":2,\"extra_days\":0,\"create_time\":\"20210329151159\",\"update_time\":\"20210329151159\"},{\"id\":\"22\",\"name\":\"测试月租B\",\"vip_type\":1,\"desc\":\"测试月租B\",\"price\":101,\"extra_days\":0,\"create_time\":\"20210329142253\",\"update_time\":\"20210329142253\"},{\"id\":\"37\",\"name\":\"储值车头\",\"vip_type\":2,\"desc\":\"储值车头\",\"price\":3,\"extra_days\":0,\"create_time\":\"20210329151207\",\"update_time\":\"20210329151207\"},{\"id\":\"52\",\"name\":\"储值挂车-H\",\"vip_type\":2,\"desc\":\"储值挂车-H\",\"price\":5,\"extra_days\":0,\"create_time\":\"20210329163302\",\"update_time\":\"20210329163302\"},{\"id\":\"61\",\"name\":\"月租挂车-H\",\"vip_type\":1,\"desc\":\"月租挂车-H\",\"price\":6,\"extra_days\":0,\"create_time\":\"20210329163307\",\"update_time\":\"20210329163307\"},{\"id\":\"1\",\"name\":\"时租车A-H\",\"vip_type\":1,\"desc\":\"时租车A-H\",\"price\":1,\"extra_days\":0,\"create_time\":\"20210329151028\",\"update_time\":\"20210329151028\"},{\"id\":\"2\",\"name\":\"时租车B-H\",\"vip_type\":1,\"desc\":\"时租车B-H\",\"price\":4,\"extra_days\":0,\"create_time\":\"20210329153052\",\"update_time\":\"20210329153052\"},{\"id\":\"3\",\"name\":\"时租车C-H\",\"vip_type\":1,\"desc\":\"时租车C-H\",\"price\":9900,\"extra_days\":0,\"create_time\":\"20210329113215\",\"update_time\":\"20210329113215\"},{\"id\":\"6\",\"name\":\"时租车F-H\",\"vip_type\":1,\"desc\":\"时租车F-H\",\"price\":10,\"extra_days\":0,\"create_time\":\"20210329145417\",\"update_time\":\"20210329145417\"},{\"id\":\"46\",\"name\":\"贵宾车-H\",\"vip_type\":0,\"desc\":\"贵宾车-H\",\"price\":0,\"extra_days\":0,\"create_time\":\"20210329165501\",\"update_time\":\"20210329165501\"}]",
  "result_code" : "1001",
  "charset" : "UTF-8",
  "message" : "操作成功",
  "version" : "1.0"
}
```

**收费类型信息:**

| 字段 | 类型 | 必须 | 说明    |
| --- | --- | --- | ---|
| id | string | Y | 车场收费规则ID. <br>对应 3.1 3.6接口里面的charge_type值|
| name | string | Y | 收费规则名称|
| vip_type| int | Y | 适用固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡<br/>0 免费停车  |
| desc | string | Y | 固定车收费描述 |
| price | int | Y | 固定车收费单价, 单位分, 若返回云端可自动配置收费模版。 |
| extra_days | int | N | 额外增送时长, 用于非时间类固定车续费自动延长有效期。 |
| create_time | string | Y | 创建时间, 格式: `yyyyMMddHHmmss` |
| update_time | string | Y | 更新时间, 格式: `yyyyMMddHHmmss` |
