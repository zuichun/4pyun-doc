# 无牌车入场

**说明:**

为实现无人值守, 针对无牌车通过扫码后平台会调用该接口完成入场开闸.若短时间重复调用该接口则不重复写入入场记录.

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.direct.enter` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| passport | string | Y | 用户通行证ID, 停车场可用作虚拟卡ID, 出场将传入, 例如: 无A123456 |
| gate_id | string | Y | 通道编号ID |
| plate | string | Y | 月卡虚拟车牌, 和passport等价, 例如: 无A123456 |
| mobile | string | N | 用户手机号 |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.direct.enter` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  操作成功.<br/>1002  未检测到车辆.<br/>1401  签名错误, 请检查配置.<br/>1403  短时间重复入场.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| enter_time | string | Y | 入场时间, 格式: `yyyyMMddHHmmss`. |
| enter_gate | string | Y | 入口名称 |
| parking_time | int | Y | 停车时长, 单位s.|
| parking_serial | string | Y | 停车流水. |
| car_type | int | Y | 车型:<br/>1. 临停车辆<br/>2. 月卡车辆<br/>3. 贵宾车辆<br/>4. 储值车辆<br/>0.其他未知 |
| car_desc | string | Y | 车类说明 |
