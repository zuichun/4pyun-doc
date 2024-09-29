# 停车费用计算

**协议说明:**

适用于外部会员对接兼容积分抵扣时长或会员时长优惠券估算折算停车费。

注意: 该接口的`free_time`和`free_value`只做费用计算，并非实际减免！！

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.payment.estimate` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| plate | string | Y | 车牌号码|
| parking_serial | string | Y | 停车流水 |
| free_time | int | Y | 减减免时间(不含已下发优惠), 单位: 分钟 |
| free_value | int | Y | 减免金额(不含已下发优惠), 单位: 分 |

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.payment.estimate` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 状态码<br/>值 含义<br/>1001  接口处理成功.<br/>1401  签名错误, 请检查配置.<br/>1403  订单已撤销.1.车辆出场2.参考7章节<br/>1500  接口内部处理失败. |
| sign | string | Y | 签名|
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| total_value | int | Y | 总停车费, 单位: 分 |
| free_value | int | N | 本次新增减免时间+金额对应总优惠金额, 单位: 分 |
| total_free_value | int | N | 本地停车总优惠金额(含本身优惠+预估新增部分), 单位: 分 |