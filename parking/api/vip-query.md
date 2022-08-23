# 车辆信息查询

**协议说明:**

P云发起向停车场, 查询指定车辆固定车信息, 停车场返回对应车牌/客户信息。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.query` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | -- | --- |
| park_uuid | string | Y | 停车场编号 |
| plate | string | Y | 绑定车牌号码. |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.query` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1002  没有查到相关固定车记录.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| vips | array | N | 固定车集合, 对象参考后面固定车信息. |

**固定车信息:**

| 字段 | 类型 | 必须 | 说明    |
| --- | --- | --- | ---|
| realname | string | Y | 客户姓名. |
| plate | string | Y | 绑定车牌号码.|
| card_no | string | N | 停车卡编号(若存在)   |
| card_id | string | N | 停车卡ID(若存在).  |
| charge_type | string | N | 收费标准类型标识; 例如:MON_A、12, 和`service.parking.charge_type.list`返回的`id`字段含义相同.|
| charge_desc | string | N | 当前固定车收费标准描述;例如: 月卡B.   |
| charge_price | int | N | 收费单价, 单位分.   |
| mobile | string | Y | 绑定的手机号.|
| id_card | string | Y | 绑定的身份证号.     |
| create_time | string | Y | 办理时间, 格式: `yyyyMMdd`  |
| expire_time | string | Y | 过期时间, 格式: `yyyyMMdd`  |
| type| int | Y | 固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡<br/>0 免费停车 |
| balance | int | Y | 当前余额<br/>储值: 为余额, 单位分;<br/>储次:为剩余次数;<br/>储天:为剩余天数;<br/>月卡/年卡/半年卡/季度卡:为剩余天数.|
| address | string | N | 车主单位/住宅 |
| park_space_no | string | N | 车位号 |
| renewal_limit_time | string | N | 限制时间类固定车续费不可超过这个最大续费日期, 格式: `yyyyMMdd` |

<font color='red'>注: 若月卡当前到期则balance=0, 若昨天过期balance=-1, 按照此规则计算</font>

*应答示例*

```json
{
  "result_code" : "1001",
  "service" : "service.parking.vip.query",
  "vips" : "[{\"balance\":0,\"card_id\":\"10760\",\"card_no\":\"100766\",\"charge_desc\":\"包月车_小型车\",\"charge_price\":20000,\"charge_type\":\"22\",\"create_time\":\"20210401090814\",\"expire_time\":\"20210401235959\",\"id_card\":\"511622199108272837\",\"mobile\":\"13632693491\",\"plate\":\"粤BN1B64\",\"realname\":\"唐泽轩\",\"type\":1}]\n",
  "charset" : "UTF-8",
  "message" : "O.K.",
  "version" : "1.0"
}
```
