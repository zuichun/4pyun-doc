# 临停订单列表查询

### 概述

此接口用于查询停车场临停订单记录。

### 接口定义

接口名称：`gate/1.0/parking/payment/list`

请求方式：`GET`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数         | 类型     | 必须 | 描述                    | 示例值             |
|------------|--------|----|-----------------------|-----------------|
| app_id     | string | Y  | 平台分配的接入应用ID           | op1234567723122 |
| sign       | string | Y  | 签名                    |                 |
| page_size  | int    | Y  | 页数                    | 10              |
| page_index | int    | Y  | 页码，从1开始               | 1               |
| park_uuid  | string | Y  | P云停车场编号               |                 |
| start_time | string | Y  | 支付开始时间，yyyyMMddHHmmss | 20240718000000  |
| end_time   | string | Y  | 支付结束时间，yyyyMMddHHmmss | 20240718235959  |
| ...        |        |    |                       |                 |

#### 示例

```json
{
  "app_id": "op00961963581daa7",
  "sign": "238CCB11D26D45E6DA81212ED77A84F6",
  "page_index": 1,
  "page_size": 10,
  "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
  "start_time": "20240718000000",
  "end_time": "20240718235959"
}
```

### 响应

#### 参数

| 参数             | 类型       | 必须 | 描述                                                           | 示例值 |
|----------------|----------|----|--------------------------------------------------------------|-----|
| id             | string   | Y  | P云订单ID                                                       |     |
| park_uuid      | string   | Y  | P云停车场编号                                                      |     |
| park_name      | string   | Y  | P云停车场名称                                                      |     |
| merchant       | string   | Y  | P云停车场商户号                                                     |     |
| plate          | string   | Y  | 车牌号码                                                         |     |
| plate_color    | int      | Y  | <a href="https://doc.4pyun.com/appendix#color_type">车牌颜色</a> |     |
| parking_serial | string   | Y  | 停车流水号                                                        |     |
| enter_time     | datetime | Y  | 入场时间                                                         |     |
| value          | int      | Y  | 订单总额, 单位分                                                    |     |
| pay_value      | int      | Y  | 支付金额, 单位分                                                    |     |
| free_value     | int      | Y  | 平台优惠金额, 单位分                                                  |     |
| discount_value | int      | Y  | 车场商户折扣金额, 单位分                                                |     |
| pay_serial     | string   | Y  | 支付单号                                                         |     |
| pay_time       | datetime | Y  | 支付时间                                                         |     |
| pay_partner    | string   | Y  | 车场订单号                                                        |     |
| ...            |          |    |                                                              |     |

#### 示例

```json
{
  "code": "1001",
  "seqno": "87368732244256431914089929792494",
  "data_node": "CN-South/DEV-3",
  "time_cost": 1441,
  "payload": {
    "paging_header": {
      "total_count": 1,
      "page_index": 1,
      "page_size": 10,
      "page_count": 1
    },
    "row": [
      {
        "id": "1020640532587614209",
        "merchant": "62626601",
        "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
        "park_name": "P云支付体验-停车场",
        "plate": "川A660N1",
        "card_no": "",
        "card_id": "",
        "ticket_formated": "",
        "enter_time": "2024-07-18T02:20:00Z",
        "parking_serial": "20247181019581721269198402",
        "pay_partner": "20240718102105024010",
        "pay_serial": "20240718102107066020110401",
        "channel": "200201",
        "pay_type": 2,
        "pay_mode": 1,
        "create_time": "2024-07-18T02:21:06Z",
        "pay_time": "2024-07-18T02:21:16Z",
        "result_time": "2024-07-18T02:21:17Z",
        "value": 1000,
        "pay_value": 1000,
        "discount_value": 0,
        "free_value": 0,
        "reduce_value": 0,
        "identity": "98DE83DD2F553EC03057A820FA60D840",
        "plate_color": -1
      }
    ]
  }
}
```

#### 响应码

| 值    | 描述             |
|------|----------------|
| 401  | 应用不存在、签名错误     |
| 403  | 接口尚未授权         |
| 1001 | 操作成功           |
| 1002 | 数据为空           |
| 1500 | 内部错误，参考message |
