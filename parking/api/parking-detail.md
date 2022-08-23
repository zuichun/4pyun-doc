# 获取停车记录详情

**协议说明:**

该协议主要用于处理异常订单, 方便P云拉取订单详情, 已方便对账以及异常订单处理.

注:最近出入场车辆查询(4.4)也返回相应停车信息以及支付信息.

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.detail` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| parking_serial | string | Y | 停车流水 |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.detail`|
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  获取成功, 业务参数将返回.<br/>1002  未查询到停车信息.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| plate | string | N | 车牌号码 |
| card_no | string | N | 支付停车卡数字编号. |
| card_id | string | N | 支付停车卡物理ID, 停车场系统识别的. |
| car_type | int | N | 车型:<br/>1. 临停车辆<br/>2. 月卡车辆<br/>3. 贵宾车辆<br/>4. 储值车辆<br/>0.其他未知 |
| car_desc | string | N | 车型类型说明 |
| vehicle_type | int | N | 车型: 1 小车, 2大车, -1未知 |
| parking_serial | string | Y | 停车流水, 标识具体某次停车事件, 需保证该停车场下唯一. |
| enter_time | string | Y | 入场时间, 格式`yyyyMMddHHmmss`. |
| enter_image | string | Y | 车辆入场照片访问地址. |
| enter_gate | string | Y | 车辆入场闸口标识. |
| enter_security | string | Y | 入场值班人员姓名. |
| parking_time | int | Y | 停车时长(单位秒) . |
| free_value | int | N | 已优惠金额(单位分) . |
| leave_time | string | N | 出场时间, 格式:`yyyyMMddHHmmss`. |
| leave_image | string | N | 车辆出场照片访问地址. |
| leave_gate | string | N | 车辆出场闸口标识. |
| leave_security | string | N | 出场值班人员姓名. |
| total_value | int | Y | 总停车费用[应收]\(单位分\) |
| payments | array | Y | 支付记录列表(包括现金支付和移动支付) |

**支付信息:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| parking_order | string | N | 停车支付订单号, 现金支付无需返回. |
| pay_serial | string | Y | 平台支付流水, 现金支付无需返回. |
| pay_type | int | Y | 支付类型: <br/>1 现金<br/>2 场中支付<br/>3 自动支付<br/>4 储值卡余额支付 |
| pay_time | string | Y | 支付时间, 格式: `yyyyMMddHHmmss`. |
| value | int | Y | 支付金额(单位分) . |
