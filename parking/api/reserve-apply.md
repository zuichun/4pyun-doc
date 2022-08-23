# 预约停车申请

**说明:**

在可预约车场提交预约停车申请, 提交的预约时间范围不表示车辆实际出入场时间, 由车场系统本地根据时间范围控制是否可入场。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.reserve.apply` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号 |
| apply_no | string | Y | 预约单号 |
| plate | string | Y | 车牌号码 |
| start_time | string | Y | 预约开始时间, 格式: `yyyyMMddHHmmss` |
| end_time | string | Y | 预约结束时间, 格式: `yyyyMMddHHmmss` |
| mobile | string | N | 联系电话 |
| realname | string | N | 车主姓名 |
| reason | string | N | 预约原因 |
| pay_serial | string | N | 预约支付流水 |
| pay_value | string | N | 支付金额, 单位分 |


**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.reserve.apply` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  预约成功.<br/>1002 无可预约车位.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
