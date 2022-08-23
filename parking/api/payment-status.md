# 停车订单查询

### 请求地址

    https://api.4pyun.com/gate/1.0/parking/internal/payment

### 调用方式

    HTTP GET

### 特殊说明:

    1. `pay_partner`本地扣费订单号, 同一个停车场唯一即使第一次失败了第二次也不能相同

### 请求参数

| 字段名称       | 字段说明                                             | 类型   | 必填 | 示例                                 |
| -------------- | ---------------------------------------------------- | ------ | :--: | ------------------------------------ |
| app_id         | 平台分配的接入应用ID, 在停车场内部使用接口时可不传递 | string |  N   | op1234567723122                      |
| api_type       | 接口类型配合app_id使用                               | string |  N   | api-debug                            |
| park_uuid      | 停车场UUID                                           | string |  Y   | 49f0cc52-e8c7-41e3-b54d-af666b8cc11a |
| pay_partner | 本地扣费订单号, 同一个停车场唯一              | string |  Y   | 111112                               |
| sign           | 请求数据签名                                         | string |  Y   | 37693CFC748049E45D87B8C7D8B9AACD     |

### 请求返回结果参数说明

| 字段名称        | 字段说明       |  类型  | 必填 | 备注                                                         |
| :-------------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code            | 请求状态码     | string |  Y   | 1001:查询成功(不表示扣款结果)<br>400:参数错误<br>500:处理失败, 提示message的信息<br>503:服务暂不可用 |
| message         | 返回描述       | string |  Y   | 返回描述                                                     |
| hint            | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno           | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| status          | 订单状态: 1=支付成功, -1=支付失败, 0=支付中  | string |  Y   | 1       |
| pay_serial      | 平台支付流水   | string |  N   | 123456789                                                    |
| pay_origin      | 到账方式       | string |  N   | 1                                                            |
| pay_origin_desc | 到账方式描述   | string |  N   | P云                                                          |


### 请求返回结果示例:


```
正常返回
{
    "code": "1001",
    "seqno": "4b1889bde31f0561",
    "data_node": "CN-South/HS3-3",
    "time_cost": 8
}
```

```
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "检查请求参数`enter_time`!",
    "seqno": "aef8bf7fe6664254",
    "data_node": "DEV/DEV-1",
    "path": "GET /gate/1.0/parking/internal/payment"
}
```

```
// 服务器内部错误
{
    "code": "500",
    "message": "服务器内部错误",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "GET /gate/1.0/parking/internal/payment"
}
```

```
// 服务器内部错误
{
    "code": "503",
    "message": "服务暂不可用",
    "hint": "NO HEALTH SERVICE(ParkingSyncer)!",
    "seqno": "7fb563f1fc2ca1f",
    "data_node": "DEV/DEV-1",
    "path": "GET /gate/1.0/parking/internal/payment"
}
```
