# 更新固定车车辆

**协议说明:**
P云发起请求, 强制更新存在车辆信息

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.update` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号|
| card_id | string | Y | 固定车卡ID |
| realname | string | N | 车主姓名 |
| mobile | string | N | 联系电话 |
| id_card | string | N | 车主身份证号码 |
| address | string | N | 车主单位/住宅 |
| park_space_no | string | N | 车位号 |
| operator | string | N | 操作员姓名 |
| start_time | string | Y | 开始时间, 格式: `yyyyMMddHHmmss` |
| end_time | string | Y | 结束时间, 格式: `yyyyMMddHHmmss` |
| plate | string | Y | 主要车牌号码     |
|vehicle| VEHICLE | Y | 车辆集合(一位多车场景) |

**VEHICLE对象**

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| plate | string | Y | 车牌号码     |
| plate_color | int | Y | 车牌颜色 |
| plate_type | int | Y | 车牌类型 |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.update` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
