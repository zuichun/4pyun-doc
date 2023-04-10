# 充电记录推送

**注意：推送必须在充电结束时进行；充电单号唯一。**

### 请求方式

`HTTP POST FORM 表单提交`

### 请求

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例值 |
|-|-|-|-|-|
| station_uuid      | string | Y    | 平台充电站编号       | 9b811f5e-df65-46a0-bec6-b56afbbbf8ea |
| device_no      | string | Y    | 本地设备编号       | DEVICE1 |
| port_no        | string | Y    | 本地设备枪号       | 1 |
| replenish_order      | string | Y    | 本地充电单号  |        |
| start_time | string | Y    | 开始充电时间戳，毫秒 | 1681122252860 |
| end_time | string | Y | 结束充电时间戳，毫秒 | 1681122252860 |
| vin | string | N   | 车辆标识。充电停车优惠场景下该值必须传递停车记录的车牌 | 川A660PP |
| quantity | string | Y    | 充电度数，0.001Kwh，1表示0.001Kwh，例如1度传递1000即可 | 1000 |
| energy_value | string | Y    | 电费，分       | 1 |
| fee_value | string | Y    | 服务费，分 |1|
| total_value | string | Y | 总金额，分 |2|
| energy_code | string | Y    | 充电类型；CN_AC：慢充；CN_DC：快充 |CN_AC|
| ...            |        |      |                  |        |

#### 示例

```json
{
    "fee_value": 561,
    "total_value": 1156,
    "quantity": "5682",
    "replenish_order": "20230410183256K7fh6t",
    "end_time": "2023-04-10T18:32:56Z",
    "sign": "4EC351C604ECB191964EB67565AA8E87",
    "device_no": "S1",
    "energy_code": "CN_AC",
    "start_time": "2023-04-10T17:32:56Z",
    "energy_value": 595,
    "vin": "川A660N2",
    "station_uuid": "8f5fdb60-9374-4c11-bdc2-a32d8369258c",
    "app_id": "op00961963581daa7",
    "timestamp": "1681122776000",
    "port_no": "1"
}
```

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| code | string | Y | 状态码 | 1001 |

#### 示例

```json
{
    "code": "1001"
}
```
