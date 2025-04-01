# 占位订单支付成功通知

## 接口描述
第三方查询到待缴占位订单，支付成功后调用此接口通知P云平台。

## 接口地址
- 测试环境：`https://dev-api.4pyun.com/gate/1.0/energy/parking/payment`
- 生产环境：`https://api.4pyun.com/gate/1.0/energy/parking/payment`

## 请求方式
`POST`
`application/x-www-form-urlencoded`

## 请求参数
| 参数            |    类型    | 是否必须 | 说明                                                                      | 备注       |
|:--------------|:--------:|:----:|-------------------------------------------------------------------------|----------|
| pay_order     |  string  |  Y   | P云平台占位单号                                                                |          |
| pay_value     |  int32   |  Y   | 支付金额，分                                                                  |          |
| pay_time      | datetime |  Y   | 实付时间                                                                    | 需传递UTC时间 |
| trade_no      |  string  |  Y   | 第三方支付单号                                                                 |          |
| pay_mode      |  string  |  Y   | <a href="https://doc.4pyun.com/openapi/appendix.html#pay_mode">到账方式</a> |          |
| pay_mode_desc |  string  |  Y   | 到账方式描述                                                                  |          |

## 响应参数
`无`

## 状态码
| 值    | 描述                         |
|------|----------------------------|
| 1001 | 成功                         |
| 1002 | 订单不存在                      |
| 1500 | 失败                         |
| 其它   | 参考 `message` 内容或 `hint` 提示 |