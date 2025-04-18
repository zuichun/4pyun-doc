# 撤销商户优惠

**协议说明:**

用于在优惠发放错误时, 清空指定停车记录已发放的商户优惠.
1. 是否是在特定时间内才可以撤回还是任意时间都能撤回？
这样有点不合理, 如果车主离开商场了, 出场时发现优惠券没了, 这时候会引起争议的;嗯, 那是否限制发出去后10分钟后不可撤销.
2. 如果发出去5分钟内车主离场了, 但是6分钟后才发觉发错了, 这时候撤不回怎么办？
如果发现已经出场, 那么你们返回无法撤销, 车辆已出场.

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.discount.destroy`|
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| grant_serial | string | Y | 优惠券派发流水.|


**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.discount.destroy` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  操作成功.<br/>1002  停车信息未找到.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |
