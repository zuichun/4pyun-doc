# 推送停止充电结果

### 请求方式

`HTTP POST FORM 表单提交`

### 请求

#### 参数

| 参数           | 类型     | 必须 | 描述                                                                             | 示例值 |
|--------------|--------|----|--------------------------------------------------------------------------------|-----|
| station_uuid | string | Y  | 站点编号                                                                           |     |
| order        | string | Y  | 充电单号                                                                           |     |
| device_no    | string | Y  | 设备编号                                                                           |     |
| port_no      | string | Y  | 设备枪号                                                                           |     |
| state        | int    | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#replenish_state">充电状态</a> |     |
| state_desc   | string | Y  | 充电状态描述                                                                         |     |

### 响应

#### 参数

| 参数   | 类型     | 必须 | 描述  | 示例   |
|------|--------|----|-----|------|
| code | string | Y  | 状态码 | 1001 |
