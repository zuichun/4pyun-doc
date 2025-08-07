# 推送充电状态

### 请求方式

`HTTP POST FORM 表单提交`

### 请求

#### 参数

| 参数             | 类型     | 必须 | 描述                                                                             | 示例值 |
|----------------|--------|----|--------------------------------------------------------------------------------|-----|
| station_uuid   | string | Y  | 站点编号                                                                           |     |
| order          | string | Y  | 充电单号                                                                           |     |
| device_no      | string | Y  | 设备编号                                                                           |     |
| port_no        | string | Y  | 设备枪号                                                                           |     |
| device_type    | short  | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#device_type">设备类型</a>     |     |
| energy_code    | string | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#energy_code">充电类型</a>     |     |
| start_time     | string | Y  | 充电开始时间，格式：yyyy-MM-dd HH:mm:ss                                                  |     |
| end_time       | string | Y  | 充电结束时间，格式：yyyy-MM-dd HH:mm:ss                                                  |     |
| replenish_time | int    | Y  | 充电时长，单位：秒                                                                      |     |
| plate          | string | Y  | 车牌号码                                                                           |     |
| quantity       | int    | Y  | 充电度数，单位：0.001Kwh                                                               |     |
| total_value    | int    | Y  | 充电总金额，单位：分                                                                     |     |
| energy_value   | int    | Y  | 充电电费总金额，单位：分                                                                   |     |
| fee_value      | int    | Y  | 充电服务费总金额，单位：分                                                                  |     |
| state          | int    | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#replenish_state">充电状态</a> |     |
| state_desc     | string | Y  | 充电状态描述                                                                         |     |

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| code | string | Y | 状态码 | 1001 |

