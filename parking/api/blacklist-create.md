# 新增黑名单车辆

**协议说明:**
P云发起请求, 将指定车辆设置为车场黑名单

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.blacklist.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号|
| plate | string | Y | 车牌号码     |
| realname | string | N | 车主姓名 |
| reason | string | N | 原因 |
| operator | string | N | 操作员姓名 |
| start_time | string | Y | 开始时间, 格式: `yyyyMMddHHmmss` |
| end_time | string | Y | 结束时间, 格式: `yyyyMMddHHmmss` |

**说明**
- 若存在记录直接更新

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.blacklist.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
