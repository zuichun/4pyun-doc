# 车场通道状态查询

**协议说明:**

平台通过该接口查询当前通道控制器状态。

**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.gate.status` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |
| gate_id | string | Y | 通道ID |


**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.gate.status` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| status | string | Y | 通道状态:<br/><b>OPEN</b> - 打开(已抬杆)<br/><b>CLOSE</b> - 关闭(已落杆)<br/><b>KEEP_OPEN</b> - 设置常开(保持打开, 不落杆) |
| status_desc | string | N | 状态说明, 例如: 设备控制常开 |
| capture_image_base64 | string | N | 实时抓拍图片JPG格式的base64编码 |
| capture_image_url | string | N | 实时抓拍图片URL |
| plate | string | N | 识别到车牌, 如果有, 例如: 粤B11111 |
| plate_color | string | N | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知 |
