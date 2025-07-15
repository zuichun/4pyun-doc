# 同步设备

## 接口描述
1. 同步设备信息至平台；
2. 设备状态变更时同步至平台等。

## 接口地址
- 测试环境：`https://dev-api.4pyun.com/gate/1.0/energy/internal/device/sync`
- 生产环境：`https://api.4pyun.com/gate/1.0/energy/internal/device/sync`

## 请求方式
`POST`
`application/json; charset=utf-8`

## 请求头
| 参数            | 说明  | 必须 | 说明 | 备注 |
|:--------------|:---:|:--:|----|----|
| Authorization | 签名值 | Y  |    |    |

## 请求参数
| 参数           |   类型   | 必须 | 说明                                                                          | 备注 |
|:-------------|:------:|:--:|-----------------------------------------------------------------------------|----|
| app_id       | string | Y  | P云平台分配的应用标识                                                                 |    |
| station_uuid | string | Y  | P云平台创建的充电站编号                                                                |    |
| no           | string | Y  | 本地设备编号                                                                      |    |
| name         | string | N  | 设备名称                                                                        |    |
| type         |  int   | N  | <a href="https://doc.4pyun.com/openapi/appendix.html#device_type">设备类型</a>  |    |
| state        |  int   | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#device_state">设备状态</a> |    |
| state_desc   | string | Y  | 设备状态描述                                                                      |    |
| energy_code  | string | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#energy_code">充电类型</a>  |    |
| port         | object | Y  | 枪口数据                                                                        |    |

`port`

| 参数           |   类型   | 必须 | 说明                                                                          | 备注         |
|:-------------|:------:|:--:|-----------------------------------------------------------------------------|------------|
|              |        |    |                                                                             |            |
| no           | string | Y  | 本地枪口编号                                                                      | 例如：DA02M01 |
| state        |  int   | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#port_state">枪口状态</a>   |            |
| state_desc   | string | Y  | 枪口状态描述                                                                      |            |
| alias        | string | Y  | 枪口别名                                                                        | 例如：A       |
| space_status |  int   | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#space_status">车位状态</a> |            |

## 响应参数
`无`

## 状态码
| 值    | 描述                         |
|------|----------------------------|
| 1001 | 成功                         |
| 1500 | 失败                         |
| 其它   | 参考 `message` 内容或 `hint` 提示 |
