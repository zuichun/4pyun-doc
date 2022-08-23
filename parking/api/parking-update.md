# 更新场内车辆信息


**说明:**

实现该接口后可在平台直接实现对场内车场对车牌、入场时间及标记离场等的更新操作。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.record.update` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| parking_serial | string | Y | 停车流水, 标识具体某次停车事件, 需保证该停车场下唯一.  |
| plate | string | N | 若传递表示需要更新车牌 |
| enter_time | string | N | 若传递表示需要更新入场时间, 格式: `yyyyMMddHHmmss`. |
| leave_time | string | N | 若传递表示需要标记离场并更新离场时间, 格式: `yyyyMMddHHmmss`. |
| reason | string | Y | 操作原因 |
| operator | string | Y | 操作人员姓名 |


**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.record.update` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  操作成功.<br/>1401  签名错误, 请检查配置.<br/>1403  短时间重复入场.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
