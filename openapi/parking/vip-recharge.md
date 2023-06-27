# 月卡续费

### 概述

此接口用于月卡续费。

### 接口定义

接口名称：`gate/1.0/parking/vip/recharge`

请求方式：`POST`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数          | 类型     | 必须 | 描述                                                         | 示例值          |
| ------------- | -------- | ---- | ------------------------------------------------------------ | --------------- |
| app_id        | string   | Y    | 平台分配的接入应用ID                                         | op1234567723122 |
| sign          | string   | Y    | 签名                                                         |                 |
| park_uuid     | string   | Y    | P云停车场编号                                                |                 |
| plate         | string   | Y    | 车牌号码                                                     |                 |
| type          | int      | Y    | <a href="https://doc.4pyun.com/appendix#vip_type">月卡类型</a> |                 |
| value         | int      | Y    | 续费面额                                                     |                 |
| quantity      | int      | Y    | 续费数量                                                     |                 |
| origin_value  | int      | Y    | 原始金额，分                                                 |                 |
| pay_value     | int      | Y    | 支付金额，分                                                 |                 |
| pay_time      | datetime | Y    | 支付时间                                                     |                 |
| trade_no      | string   | Y    | 支付单号                                                     |                 |
| channel       | string   | Y    | 支付渠道                                                     |                 |
| pay_type      | int      | Y    | <a href="https://doc.4pyun.com/appendix#pay_type">支付类型</a> |                 |
| pay_mode      | int      | Y    | <a href="https://doc.4pyun.com/appendix#pay_mode">支付到账模式</a> |                 |
| pay_mode_desc | string   | Y    | 支付到账模式描述                                             |                 |
| ...           |          |      |                                                              |                 |

#### 示例

```json
{
    "origin_value": "1",
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "pay_mode_desc": "城市平台测试支付",
    "quantity": "1",
    "pay_value": "1",
    "sign": "6E4E3A0C604163049711D2D67FFDF9E2",
    "channel": "0",
    "plate": "川A660P1",
    "type": "1",
    "pay_time": "2023-06-26T10:12:40.301Z",
    "pay_mode": "25",
    "trade_no": "20230626181240",
    "pay_type": "2",
    "value": "31",
    "app_id": "op00961963581daa7"
}
```

### 响应

#### 参数

`无`

#### 示例

```json
{
    "code": "1000",
    "seqno": "64380142324163870912631629047433",
    "data_node": "CN-South/DEV-1",
    "time_cost": 65
}
```

#### 响应码

| 值   | 描述                  |
| ---- | --------------------- |
| 401  | 应用不存在、签名错误  |
| 403  | 接口尚未授权          |
| 1000 | 操作受理              |
| 1500 | 内部错误，参考message |
