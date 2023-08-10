# 支付渠道配置信息查询接口

```
GET https://api.4pyun.com/gate/1.0/payment/channel/config
```

**接口说明**

	该接口通过P云平台商户号查询该商户对应的支付渠道配置信息


**请求参数**

| 字段     | 类型   | 必须 | 说明                 |
| -------- | ------ | ---- | -------------------- |
| app_id   | string | Y    | 平台分配的接入应用ID |
| sign     | string | Y    | 请求数据签名         |
| merchant | string | Y    | 商户号               |


**公共响应参数**

| 字段      | 类型   | 必须 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| code      | string | Y    | 业务处理状态码<br>1001:查询成功<br/>1002:结算帐户不存在<br/>其它状态码:读取message |
| message   | string | N    | 业务处理状态说明                                             |
| hint      | string | N    | 提示说明                                                     |
| seqno     | string | Y    | 请求编号，用于排查定位请求问题                               |
| data_node | string | Y    | 请求服务器编码                                               |
| time_cost | long   | Y    | 请求响应耗时，单位毫秒                                       |
| payload   | object | N    | 响应业务参数                                                 |



**业务响应参数**

| payload字段 | 类型 | 必须 | 说明             |
| ----------- | ---- | ---- | ---------------- |
| row         | list | Y    | 支付渠道配置列表 |



| row字段        | 类型   | 必须 | 说明                                                         |
| -------------- | ------ | ---- | ------------------------------------------------------------ |
| type_id        | short  | N    | 支付渠道类型ID                                               |
| name           | string | N    | 支付渠道类型名称                                             |
| status         | short  | N    | 支付渠道配置状态: 0 未配置; 1 待审核; 2 审核中; 5 代验证; 4 待签约; 3 审核通过; |
| apply_no       | string | N    | 支付渠道申请编号                                             |
| apply_sign_url | string | N    | 签约链接                                                     |
| abnormal       | short  | N    | 是否帐户异常: 0 正常; 1异常;                                 |
| category_id    | short  | Y    | 支付渠道分类ID                                               |
| category_name  | string | Y    | 支付渠道分类名称                                             |
| type_code      | string | N    | 支付渠道类型编码                                             |
| create_time    | string | N    | 支付渠道申请时间                                             |
| update_time    | string | N    | 支付渠道更新时间                                             |
| status_desc    | string | N    | 异常状态描述                                                 |


**请求返回结果示例**

```
{
    "code": "1001",
    "seqno": "83980312925941224530392031515874",
    "data_node": "CN-South/DEV-1",
    "time_cost": 34,
    "payload": {
        "row": [
            {
                "type_id": 0,
                "name": "",
                "status": 0,
                "apply_no": "",
                "apply_sign_url": "",
                "abnormal": 0,
                "category_id": "1",
                "category_name": "微信直连",
                "type_code": "",
                "create_time": "1970-01-01T00:00:00Z",
                "abnormal_type": 0,
                "update_time": "1970-01-01T00:00:00Z",
                "status_desc": ""
            },
            {
                "type_id": 0,
                "name": "",
                "status": 0,
                "apply_no": "",
                "apply_sign_url": "",
                "abnormal": 0,
                "category_id": "2",
                "category_name": "支付宝直连",
                "type_code": "",
                "create_time": "1970-01-01T00:00:00Z",
                "abnormal_type": 0,
                "update_time": "1970-01-01T00:00:00Z",
                "status_desc": ""
            },
            {
                "type_id": 5,
                "name": "聚合支付",
                "status": 3,
                "apply_no": "C202304140633",
                "apply_sign_url": "https://dev-app.4pyun.com/contract/prepare?contract_no=DEV-P061202211160001",
                "abnormal": 1,
                "category_id": "3",
                "category_name": "聚合支付",
                "type_code": "PPPAY",
                "create_time": "2023-04-14T08:18:49Z",
                "abnormal_type": 1,
                "update_time": "2023-06-15T13:25:33Z",
                "status_desc": "户名有误"
            },
            {
                "type_id": 23,
                "name": "中国邮政支付",
                "status": -3,
                "apply_no": "C202208184494",
                "apply_sign_url": "",
                "abnormal": 0,
                "category_id": "4",
                "category_name": "银行聚合支付",
                "type_code": "",
                "create_time": "2022-08-18T08:00:01Z",
                "abnormal_type": 0,
                "update_time": "2023-04-06T03:43:00Z",
                "status_desc": "已停用"
            }
        ]
    }
}
```