# 占位订单查询

## 接口描述
通过车牌查询在该场站内待缴费的占位订单。

## 接口地址
- 测试环境：`https://dev-api.4pyun.com/gate/1.0/energy/parking/payment/billing`
- 生产环境：`https://api.4pyun.com/gate/1.0/energy/parking/payment/billing`

## 请求方式
`GET`
`application/x-www-form-urlencoded`

## 请求参数
| 参数       |   类型   | 必须 | 说明    | 备注 |
|:---------|:------:|:--:|-------|----|
| merchant | string | Y  | 站点商户号 |    |
| plate    | string | Y  | 车牌号码  |    |

## 响应参数
| 参数           |    类型    | 必须 | 说明                                                                        | 备注                                                             |
|:-------------|:--------:|:--:|---------------------------------------------------------------------------|----------------------------------------------------------------|
| merchant     |  string  | Y  | 站点商户号                                                                     |                                                                |
| station_uuid |  string  | Y  | 站点编号                                                                      |                                                                |
| station_name |  string  | Y  | 站点名称                                                                      |                                                                |
| plate        |  string  | Y  | 车牌号码                                                                      |                                                                |
| plate_color  |  int32   | Y  | <a href="https://doc.4pyun.com/openapi/appendix.html#color_type">车牌颜色</a> |                                                                |
| space_no     |  string  | Y  | 车位号                                                                       |                                                                |
| enter_time   | datetime | Y  | 入场时间                                                                      | UTC时间。例如 `2025-02-27T08:06:24Z` 转换为北京时间为 `2025-02-27 16:06:24` |
| enter_image  |  string  | N  | 入场图片                                                                      |                                                                |
| leave_time   | datetime | Y  | 离场时间                                                                      | UTC时间。例如 `2025-02-27T08:06:24Z` 转换为北京时间为 `2025-02-27 16:06:24` |
| leave_image  |  string  | N  | 离场图片                                                                      |                                                                |
| parking_time |  int32   | Y  | 停车时长，秒                                                                    |                                                                |
| pay_order    |  string  | Y  | 占位单号                                                                      |                                                                |
| total_value  |  int32   | Y  | 总金额，分                                                                     |                                                                |
| need_value   |  int32   | Y  | 需支付金额，分                                                                   |                                                                |

## 状态码
| 值    | 描述                         |
|------|----------------------------|
| 1001 | 成功                         |
| 1002 | 暂无待缴费的占位订单                 |
| 1500 | 失败                         |
| 其它   | 参考 `message` 内容或 `hint` 提示 |