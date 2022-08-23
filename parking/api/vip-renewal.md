# 车辆续费通知

**协议说明:**
P云发起请求, 对固定车进行充值续费。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.renewal` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号|
| plate | string | N | 绑定车牌号码.     |
| card_no | string | N | 停车卡编号(若存在)  |
| card_id | string | N | 停车卡ID(若存在). |
| pay_time | string | Y | 续费时间, 格式: `yyyyMMddHHmmss`|
| pay_serial | string | Y | 支付流水, 用于对账. |
| pay_value | int | Y | 客户支付金额, 单位分.|
| type| int | Y | 固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡<br/>0 免费停车  |
| value | int | Y | 续费金额:<br/>储值: 续费金额, 单位分;<br/>储次:续费次数;<br/>储天:续费天数;<br/>月卡/年卡/半年卡/季度卡: 续费天数. |
| quantity | int | Y | 续费数量:<br/>储值: 续费金额, 单位分;<br/>储次:续费次数;<br/>储天:续费天数;<br/>月卡:续费月数;<br/>年卡:续费年数;<br/>半年卡:续费半年数;<br/>季度卡:续费季度数.|
| pay_origin | int | Y | 值 含义<br/>0    P云<br/>4   支付宝<br/>8    微信<br/>-     兼容其他未定义 |
| pay_origin_desc | string | Y | 支付来源说明, 例如:P云 |
| pay_source | string | N | 付款来源, 例如: 微信/支付宝/银联 |
| renewal_start_time | string | Y | 时间类续费开始时间, 格式: `yyyyMMddHHmmss` |
| renewal_end_time | string | Y | 时间类续费结束时间, 格式: `yyyyMMddHHmmss` |

**说明**

1. 实现该接口如果是时间类固定车，建议直接按照`renewal_start_time`~`renewal_end_time`插入新的有效时间, 可实现过期续费; 例如：月卡A当前有效期为`2019-01-01~2019-10-31`:

- 若在`2019-10-31`之前续费一个月，则接口下发的时间范围为: `2019-11-01～2019-11-30`,
- 若在`2019-11-05`续费, 月卡已过期，若停车场允许过期7天内续费，则续费后有两种时间计算方式：
  - **从过期时间开始** - 则接口下发时间范围为: `2019-11-01~2019-11-30`;
  - **从续费时间开始计算** - 则接口下发时间范围为: `2019-11-05~2019-12-05`。

2. 车场本地其他固定车计算延期方式不是固定的可根据下发的参数自己选择适合自己的计算方式。

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.renewal` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1002  没有查到相关固定车记录.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
