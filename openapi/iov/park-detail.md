# 停车场详情查询

### 概述

此接口用于查询 **已授权 ** 的停车场详情。

### 接口定义

接口名称：`gate/1.0/parking/park`

请求方式：`GET`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数      | 类型   | 必须 | 描述                 | 示例值          |
| --------- | ------ | ---- | -------------------- | --------------- |
| app_id    | string | Y    | 平台分配的接入应用ID | op1234567723122 |
| sign      | string | Y    | 签名                 |                 |
| park_uuid | string | Y    | 车场标识             |                 |
| ...       |        |      |                      |                 |

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "sign": "B1DA7B0D527A53B5B3AF68BB26CAE48D",
    "app_id": "op00961963581daa7"
}
```

### 响应

#### 参数

| 参数               | 类型     | 必须 | 描述                                                         | 示例值                                    |
| ------------------ | -------- | ---- | ------------------------------------------------------------ | ----------------------------------------- |
| park_uuid          | string   | Y    | P云车场标识                                                  |                                           |
| park_name          | string   | Y    | P云车场名称                                                  | P云支付体验-停车场                        |
| address            | string   | Y    | 详细地址                                                     | 广东省深圳市南山区深圳湾科技生态园10栋    |
| latitude           | double   | Y    | 纬度                                                         | 22.535853                                 |
| longitude          | double   | Y    | 经度                                                         | 113.958233                                |
| coord_type         | string   | Y    | <a href="https://doc.4pyun.com/appendix#coord_type">坐标系类型</a>，默认：WGS84 | WGS84                                     |
| province           | string   | Y    | 省                                                           | 广东省                                    |
| city               | string   | Y    | 市                                                           | 深圳市                                    |
| district           | string   | Y    | 区                                                           | 南山区                                    |
| area_code          | string   | Y    | 区域编码                                                     | 440305                                    |
| distance           | int      | Y    | 距离，米                                                     | 10                                        |
| total_space        | int      | Y    | 总车位数                                                     | 100                                       |
| remain_space       | int      | Y    | 剩余车位数                                                   | 30                                        |
| charge_desc        | string   | Y    | 收费规则描述                                                 | 首小时4元, 之后1元/小时, 全天最高收费10元 |
| charge_simple_desc | string   | Y    | 收费规则简单描述                                             | 首小时4元,之后1元/小时                    |
| charge_depict      | string   | Y    | 收费规则JSON数据                                             |                                           |
| enter_free_time    | int      | Y    | 入场免费时间，分钟                                           | 30                                        |
| leave_buffer_time  | int      | Y    | 出场预留时间，分钟                                           | 10                                        |
| image_url          | string[] | Y    | 车场图片集合                                                 |                                           |
| tag                | string[] | Y    | <a href="https://doc.4pyun.com/appendix#park_tag">车场标签集合</a> |                                           |
| ...                |          |      |                                                              |                                           |

#### 示例

```json
{
    "code": "1001",
    "seqno": "80381443062105966129090376032528",
    "data_node": "CN-South/DEV-1",
    "time_cost": 44,
    "payload": {
        "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
        "merchant": "",
        "park_name": "P云支付体验-停车场",
        "address": "广东省深圳市南山区沙河西路辅路与高新南环路交汇处西南角深圳湾科技生态园",
        "latitude": 22.53288059,
        "longitude": 113.9476602,
        "province": "广东省",
        "city": "深圳市",
        "district": "南山区",
        "area_code": "440305",
        "total_space": 300,
        "remain_space": 2,
        "realtime": 0,
        "first_charge_time": 0,
        "first_charge_value": 0,
        "charge_desc": "首小时4元,之后1元/小时，过夜收费40元",
        "charge_simple_desc": "首小时4元,之后1元/小时",
        "charge_extra_desc": "",
        "enter_free_time": 0,
        "leave_buffer_time": 1,
        "image_url": [
            
        ],
        "tag": [
            "测试"
        ],
        "coord_type": "WGS84",
        "accept_recharge": 0,
        "corp_status": 0
    }
}
```

