# 推送固定车续费

**注意：推送必须基于有效的车牌白名单。**

### 请求方式

`HTTP POST FORM 表单提交`

### 请求

#### 参数

| 参数       | 类型   | 必须 | 描述                     | 示例值                                         |
| ---------- | ------ | ---- | ------------------------ |-|
| park_uuid  | string | Y    | 停车场编号               |                                                |
| park_name  | string | Y    | 停车场名称               |                                                |
| pay_serial | string | Y    | 平台停车流水             |                                                |
| pay_value  | string | Y    | 支付金额，分             |                                                |
| pay_time   | string | Y    | 支付时间戳，毫秒         |                                                |
| plate      | string | Y    | 车牌                     |                                                |
| quantity   | string | Y    | 续费数量                 | 时间类：31 表示31天<br />非时间类：31 表示31分 |
| vip_type   | string | Y    | 月卡类型                 |                                                |
| start_time | string | Y    | 时间类固定车续费开始时间 |                                                |
| end_time   | string | Y    | 时间类固定车续费结束时间 |                                                |
| channel    | string | Y    | 支付渠道                 |                                                |
| identity   | string | Y    | 平台用户标识             |                                                

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "pay_serial": "20210421164242075523115813",
    "pay_value": "100",
    "sign": "B1BAA9B6C83DB3E0C50F84B7B54AC848",
    "app_id": "op00961963581daa7",
    "sign_type": "MD5",
    "park_name": "P云支付体验-停车场",
    "timestamp": "1618994562891",
    "pay_time": "1618994563000",
    "plate": "川A660P1",
    "quantity": "31",
    "vip_type": "1",
    "start_time": "1618370771000",
    "end_time": "1620962771556",
    "channel": "200102",
    "identity": "5aeV05nlt6/ig4LplITisYg="
}
```

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| code | string | Y | 状态码 | 001 |

#### 示例

```
{
    "code": "1001"
}
```
