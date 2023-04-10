# 充电记录推送

### 请求地址

```
https://api.4pyun.com/gate/1.0/energy/internal/replenish
```

### 调用方式

```
HTTP FORM 表单提交
```

### 特殊说明

**注意事项：**

1. 需在充电结束时触发调用；
2. 一次充电单号唯一；
3. 下发停车优惠时 `vin` 需上传车牌；


### 请求参数

| 参数 | 类型 | 必须 | 描述 | 示例值 |
|-|-|-|-|-|
| app_id | string |  Y | 平台分配的接入应用ID | op1234567723122 |
| timestamp | string |  Y | 请求发送时间戳(ms), 用于防重放攻击服务器直接受和服务器时间差10分钟内的请求。 | 1552976318722 |
| sign | string | Y | 请求数据签名 | C65FCAC2D3FB5E2D3D4AD93DD20C8C39  |
| station_uuid      | string | Y    | 平台充电站编号       | 9b811f5e-df65-46a0-bec6-b56afbbbf8ea |
| device_no      | string | Y    | 本地设备编号       | DEVICE1 |
| port_no        | string | Y    | 本地设备枪号       | 1 |
| replenish_order      | string | Y    | 本地充电单号  |        |
| start_time | string | Y    | 开始充电，`yyyy-MM-dd'T'HH:mm:ss'Z'` | 2023-04-10T17:37:30Z |
| end_time | string | Y | 结束充电，`yyyy-MM-dd'T'HH:mm:ss'Z'` | 2023-04-10T17:37:30Z |
| vin | string | N   | 车辆标识。充电停车优惠场景下该值必须传递停车记录的车牌 | 川A660PP |
| quantity | string | Y    | 充电度数，0.001Kwh，1表示0.001Kwh，例如1度传递1000即可 | 1000 |
| energy_value | string | Y    | 电费，分       | 1 |
| fee_value | string | Y    | 服务费，分 |1|
| total_value | string | Y | 总金额，分 |2|
| energy_code | string | Y    | 充电类型；CN_AC：慢充；CN_DC：快充 |CN_AC|
| ...            |        |      |                  |        |

### 请求示例

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

### 响应参数
| 字段名称 | 字段说明 |  类型  | 必填 | 备注  |
| :--- | :--- | :---: | :--: | :--- |
| code  | 请求状态码  | string |  Y   | 200:成功受理<br> 400:参数错误<br> 403:访问被拦截<br>500:服务器内部错误<br>503:服务暂不可用 |
| message  | 返回描述 | string |  Y   | 返回描述 |
| hint  | 返回错误说明   | string |  N   | 返回具体错描述指导|
| seqno | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值|


### 响应示例

> 正常返回

```json
{
    "code": "200",
    "message": "OK",
    "seqno": "abc6c253b93cc940",
    "data_node": "CN-South/HS3-1",
    "vehicle_violation_count": 0,
    "vehicle_inspect_state": 1,
    "vehicle_insurance_state": 1
}
```

> 正常返回 带hint 对接过程中要特别注意这种情况，正式上线了这种也直接视为成功，防止一直卡在一条数据上面

```json
{
    "code": "200",
    "message": "已忽略当前请求",
    "hint": "签名验证不通过",
    "seqno": "e820972bdf2eaf19",
    "data_node": "CN-South/HS3-3",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 参数错误

```json
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "`start_time` required~",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 服务器内部错误

```json
{
    "code": "500",
    "message": "服务器内部错误",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 服务器内部错误

```json
{
    "code": "503",
    "message": "服务暂不可用",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```
