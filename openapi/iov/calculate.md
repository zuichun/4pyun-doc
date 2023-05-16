## 根据车牌查询停车信息并试算费用

### 概述

此接口用于查询 **已授权 ** 的停车场，车辆停车订单信息并 **试算** 费用。

### 接口定义

接口名称：`gate/1.0/parking/calculate`

请求方式：`GET`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数      | 类型   | 必须 | 描述                 | 示例值          |
| --------- | ------ | ---- | -------------------- | --------------- |
| app_id    | string | Y    | 平台分配的接入应用ID | op1234567723122 |
| sign      | string | Y    | 签名                 |                 |
| park_uuid | string | Y    | 车场编号             |                 |
| plate     | string | Y    | 车牌号码             |                 |
| ...       |        |      |                      |                 |

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "sign": "44CAF15ED1CDED9D5072F83A9997775F",
    "plate": "川A660N1",
    "app_id": "op00961963581daa7"
}
```

### 响应

#### 参数

| 参数            | 类型   | 必须 | 描述                               | 示例值             |
| --------------- | ------ | ---- | ---------------------------------- | ------------------ |
| park_uuid       | string | Y    | 车场编号                           |                    |
| park_name       | string | Y    | 车场名称                           | P云支付体验-停车场 |
| plate           | string | Y    | 车牌号码                           | 川A660PP           |
| ticket_formated | string | Y    | 停车小票号                         |                    |
| card_id         | string | Y    | 停车卡ID                           |                    |
| enter_time      | string | Y    | 入场时间，yyyy-MM-dd'T'HH:mm:ss'Z' |                    |
| parking_time    | int    | Y    | 当前停车时长，秒                   | 100                |
| free_time       | int    | Y    | 优惠时长，秒                       | 0                  |
| total_value     | int    | Y    | 总停车费，分                       | 100                |
| free_value      | int    | Y    | 免费金额，分                       | 0                  |
| need_value      | int    | Y    | 需支付金额，分                     | 1                  |
| paid_value      | int    | Y    | 已支付金额，分                     | 0                  |
| buffer_time     | int    | Y    | 支付后预留出场时间，分钟           | 0                  |
| enter_free_time | int    | Y    | 入场免费时间，分钟                 | 0                  |
| car_type        | int    | Y    | 车型                               | 1                  |
| car_desc        | string | Y    | 车型说明                           | 临时车             |
| parking_id      | string | Y    | 平台停车记录ID                     |                    |
| ...             |        |      |                                    |                    |

#### 示例

```json
{
    "code": "1001",
    "seqno": "66181277507387231645816561926061",
    "data_node": "CN-South/DEV-1",
    "time_cost": 934,
    "payload": {
        "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
        "park_name": "P云支付体验-停车场",
        "plate": "川A660N1",
        "ticket_formated": "",
        "card_id": "",
        "enter_time": "2023-05-16T02:24:25Z",
        "parking_time": 2113,
        "free_time": 0,
        "total_value": 1,
        "free_value": 0,
        "need_value": 1,
        "paid_value": 0,
        "buffer_time": 0,
        "enter_free_time": 0,
        "parking_id": "865176976783052801",
        "car_type": 1,
        "car_desc": "临时车"
    }
}
```

