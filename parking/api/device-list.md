# 车场设备列表


**协议说明:**

P云在需要停车场本地文件业务处理时通过本协议获取指定文件.


**请求参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.device.list` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| sign | string | Y | 签名|

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| park_uuid | string | Y | 停车场编号 |


**应答参数:**

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| service | string | Y | 服务名: `service.parking.device.list` |
| version | string | Y | 版本号:`1.0`|
| charset | string | Y | 字符集:`UTF-8`|
| result_code | string | Y | 本次请求状态返回码, 示例:1001<br/>状态码  含义<br/>1001  接口处理成功, 业务参数将返回.<br/>1401  签名错误, 请检查配置.<br/>1500  接口处理异常. |
| sign | string | Y | 当前请求应答结果, 签名方法参考附录. |
| message | string | Y | 状态码处理描述, 如:返回错误信息. |

| 字段 | 类型 | 必须 | 说明|
| --- | --- | --- | --- |
| device_list | array | N | 设备集合, 对象参考后面设备信息. |

**设备信息:**

| 字段 | 类型 | 必须 | 说明    |
| --- | --- | --- | ---|
| name | string | Y | 设备名称 |
| type | int | Y | 设备类型:<br/>1 相机<br/>2 入口通道控制器<br/>3 出口通道控制器<br/> 4 出入口通道控制器 |
| no | string | N | 设备编号 |
| uuid | string | N | 设备ID |
| vendor | string | N | 制造商, 例如: 大华/海康 |
| model | string | N | 设备型号, 例如: TZ-O1 |
| status | int | Y | 状态:<br/> -1 已离线<br/> 0 未知<br/> 1 正常运行 |
| status_desc | string | Y | 状态描述, 例如: 脱机 |
| mac_addr | string | N | 网卡MAC地址, 例如: ac:de:48:a0:11:24 |
| ipv4_addr | string | N | 网卡IP地址, 例如: 172.12.1.29 |
| uptime | string | N | 启动时间, 格式`yyyyMMddHHmmss`.|
| timestamp | string | N | 设备时间, 格式`yyyyMMddHHmmss`.|
