# 上报设备状态

### 请求地址

```
https://api.4pyun.com/gate/1.0/energy/internal/device
```

### 调用方式

```
HTTP FORM 表单提交
```

### 请求参数

| 参数              | 类型     | 必须 | 描述                                                                          | 示例值                              |
|-----------------|--------|----|-----------------------------------------------------------------------------|----------------------------------|
| app_id          | string | Y  | 平台分配的接入应用ID                                                                 | op1234567723122                  |
| timestamp       | string | Y  | 请求发送时间戳(ms), 用于防重放攻击服务器直接受和服务器时间差10分钟内的请求。                                  | 1552976318722                    |
| sign            | string | Y  | 请求数据签名                                                                      | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| station_uuid    | string | Y  | PP充电平台充电站UUID（需先在PP平台创建充电站才可获得）                                             |                                  |
| no              | string | Y  | 本地设备编号（传递本地设备编号即可）                                                          | DEVICE1                          |
| state           | int    | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#device_state">设备状态</a> | 1                                |
| state_desc      | string | Y  | 设备状态描述                                                                      |                                  |
| ...             |        |    |                                                                             |                                  |

### 响应参数
| 字段名称    | 字段说明    |   类型   | 必填 | 备注              |
|:--------|:--------|:------:|:--:|:----------------|
| code    | 请求状态码   | string | Y  | 1001            |
| message | 返回描述    | string | Y  | 返回描述            |
| hint    | 返回错误说明  | string | N  | 返回具体错描述指导       |
| seqno   | 服务器日志标示 | string | Y  | 查日志用到查问题尽量提供这个值 |


### 响应码
| 值    | 描述 |
|------| --- |
| 1001 | 成功 |
| 其它   | 参考message |
