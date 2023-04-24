# 预约停车申请

### 1.1) 请求地址

 https://api.4pyun.com/gate/1.0/parking/reserve/apply

### 1.2) 调用方式

 HTTP POST FORM 表单提交

### 1.3) 特殊说明

 1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请


### 1.4) 请求参数

| 字段名称   | 字段说明             |  类型  | 必填 | 示例                             |
| :--------- | :------------------- | :----: | :--: | :------------------------------- |
| app_id     | 平台分配的接入应用ID | string |  Y   | op1234567723122                  |
| sign       | 请求数据签名         | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| park_uuid   | P云停车场编号 | string |  Y   | 49f0cc52-e8c7-41e3-b54d-af666b8cc11a |
| plate      | 车牌号码             | string |  Y   | 川A660P2                         |
| plate_color      | 车牌颜色             | string |  Y   | 1 |
| realname      | 姓名             | string |  Y   | 张三 |
| mobile      | 手机号码             | string |  Y   | 13800138001 |
| reason     | 原因             | string |  Y   | 测试                             |
| start_time | 开始时间             | string |  Y   | 2020-12-31T16:00:15Z             |
| end_time   | 结束时间             | string |  Y   | 2020-12-31T16:00:15Z             |


### 1.5) 响应参数

| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                |
| :------- | :------------- | :----: | :--: | :-------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:添加成功<br>400:参数错误<br>500:服务器内部错误 |
| message  | 返回描述       | string |  N   | 返回描述                                            |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                  |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                      |
| time_cost | 接口响应时间 | int |  Y   | 1768 |
| payload    | 响应对象 | object |  N   |  |


### 1.6) 响应示例:

```json
{
    "code": "1001",
    "seqno": "98930483256240241867333823825944",
    "data_node": "CN-South/DEV-1",
    "time_cost": 1768,
    "payload": {
        "apply_no": "20230420101801447771267107"
    }
}
```
