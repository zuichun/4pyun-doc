# 月卡查询

### 概述

此接口用于查询月卡信息。

### 接口定义

接口名称：`gate/1.0/parking/vip`

请求方式：`GET`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数      | 类型   | 必须 | 描述                 | 示例值          |
| --------- | ------ | ---- | -------------------- | --------------- |
| app_id    | string | Y    | 平台分配的接入应用ID | op1234567723122 |
| sign      | string | Y    | 签名                 |                 |
| park_uuid | string | Y    | P云停车场编号        |                 |
| plate     | string | Y    | 车牌号码             |                 |
| ...       |        |      |                      |                 |

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "sign": "E8A0AD3C6CA62135E101283D0658EC59",
    "plate": "川A660P1",
    "app_id": "op00961963581daa7"
}
```

### 响应

#### 参数

| 参数            | 类型     | 必须 | 描述                                                         | 示例值 |
| --------------- | -------- | ---- | ------------------------------------------------------------ | ------ |
| plate           | string   | Y    | 车牌号码                                                     |        |
| type            | int      | Y    | <a href="https://doc.4pyun.com/appendix#vip_type">月卡类型</a> |        |
| balance         | int      | Y    | 余额                                                         |        |
| create_time     | datetime | Y    | 发卡时间                                                     |        |
| expire_time     | datetime | Y    | 过期时间                                                     |        |
| realname        | string   | Y    | 姓名                                                         |        |
| id_card         | string   | Y    | 身份证                                                       |        |
| mobile          | string   | Y    | 手机号码                                                     |        |
| charge_type     | string   | Y    | 月卡收费类型                                                 |        |
| charge_desc     | string   | Y    | 月卡收费描述                                                 |        |
| charge_price    | int      | Y    | 月卡收费单价，分                                             |        |
| accept_apply    | int      | Y    | 是否允许开卡                                                 |        |
| accept_recharge | int      | Y    | 是否允许续费                                                 |        |
| ...             |          |      |                                                              |        |

#### 示例

```json
{
    "code": "1001",
    "seqno": "65847430109545664686923431510983",
    "data_node": "CN-South/DEV-1",
    "time_cost": 39,
    "payload": {
        "plate": "川A660P1",
        "card_no": "",
        "card_id": "",
        "create_time": "2022-09-22T07:54:43Z",
        "expire_time": "2027-05-19T15:59:59Z",
        "type": 1,
        "balance": 1423,
        "realname": "小明",
        "mobile": "13923436192",
        "id_card": "",
        "charge_type": "1",
        "charge_desc": "我是月卡A的描述哦~~~",
        "charge_price": 1000,
        "vip_id": "0",
        "accept_apply": 1,
        "accept_recharge": 1
    }
}
```

#### 响应码

| 值   | 描述                  |
| ---- | --------------------- |
| 401  | 应用不存在、签名错误  |
| 403  | 接口尚未授权          |
| 1001 | 操作成功              |
| 1002 | 月卡不存在            |
| 1500 | 内部错误，参考message |
