# 新增固定车车辆

**协议说明:**
P云发起请求, 将指定车辆设置为车场固定车

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | ---|
| park_uuid | string | Y | 停车场编号|
| plate | string | Y | 车牌号码     |
| realname | string | N | 车主姓名 |
| mobile | string | N | 联系电话 |
| id_card | string | N | 车主身份证号码 |
| card_id | string | N | 固定车卡ID |
| start_time | string | Y | 开始时间, 格式: `yyyyMMddHHmmss` |
| end_time | string | Y | 结束时间, 格式: `yyyyMMddHHmmss` |
| charge_type | string | Y | 收费类型 |
| value | int | Y | 续费金额:<br/>储值: 续费金额, 单位分;<br/>储次:续费次数;<br/>储天:续费天数;<br/>月卡/年卡/半年卡/季度卡: 续费天数. |
| quantity | int | Y | 续费数量:<br/>储值: 续费金额, 单位分;<br/>储次:续费次数;<br/>储天:续费天数;<br/>月卡:续费月数;<br/>年卡:续费年数;<br/>半年卡:续费半年数;<br/>季度卡:续费季度数.|
| origin_value | int | Y | 原始价格, 单位分 |
| pay_value | int | Y | 用户实际付款金额, 单位分 |
| pay_origin | int | Y | 值 含义<br/>0    P云<br/>4   支付宝<br/>8    微信<br/>-     兼容其他未定义|
| pay_origin_desc | string | Y | 支付到账说明, 例如:P云|
| pay_source | string | N | 付款来源, 例如: 微信/支付宝/银联 |
| address | string | N | 车主单位/住宅 |
| park_space_no | string | N | 车位号 |
| operator | string | N | 操作员姓名 |

**说明**
- 若存在记录则返回`1403`拒绝创建!

**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.vip.create` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1403 固定车已存在拒绝创建<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
