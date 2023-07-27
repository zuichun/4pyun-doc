# 结算帐户信息查询接口

### 1.1) 请求方式

	https://api.4pyun.com/gate/1.0/payment/channel/bank/account

### 1.2) 请求方式
	HTTP GET

### 1.3) 接口说明

	该接口通过P云平台商户号查询该商户对应的结算帐户信息



### 1.4)请求参数

| 字段     | 类型   | 必须 | 说明                 |
| -------- | ------ | ---- | -------------------- |
| app_id   | string | Y    | 平台分配的接入应用ID |
| sign     | string | Y    | 请求数据签名         |
| merchant | string | Y    | 商户号               |



### 1.5) 公共响应参数

| 字段      | 类型   | 必须 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| code      | string | Y    | 业务处理状态码<br>1001:查询成功<br/>1002:结算帐户不存在<br/>其它状态码:读取message |
| message   | string | N    | 业务处理状态说明                                             |
| hint      | string | N    | 提示说明                                                     |
| seqno     | string | Y    | 请求编号，用于排查定位请求问题                               |
| data_node | string | Y    | 请求服务器编码                                               |
| time_cost | long   | Y    | 请求响应耗时，单位毫秒                                       |
| payload   | object | N    | 响应业务参数                                                 |



### 1.6) 业务响应参数

| 字段               | 类型   | 必须 | 说明                                                         |
| ------------------ | ------ | ---- | ------------------------------------------------------------ |
| merchant           | string | Y    | 商户号                                                       |
| merchant_name      | string | Y    | 商户名称                                                     |
| payee_name         | string | Y    | 收款帐号主体名称                                             |
| payee_account      | string | Y    | 收款帐号                                                     |
| payee_type         | string | Y    | 收款帐号类型                                                 |
| bank_name          | string | Y    | 银行名称                                                     |
| bank_province      | string | Y    | 支行所属省份                                                 |
| bank_city          | string | Y    | 支行所属城市                                                 |
| bank_branch        | string | Y    | 支行名称                                                     |
| id                 | long   | Y    | 结算帐户ID                                                   |
| status             | short  | Y    | 结算帐户状态：1 帐户正常；0 帐户异常；                       |
| status_desc        | string | N    | 帐户状态描述，帐户异常则为异常原因描述                       |
| subject_template   | string | Y    | 打款备注信息模版                                             |
| bank_code          | string | Y    | 支行联行号                                                   |
| withdraw_type      | short  | Y    | 划账模式：0 自动划账;                                        |
| settle_cycle       | string | Y    | 划账周期                                                     |
| min_transfer_value | string | Y    | 最小划账金额，单位分                                         |
| priority           | string | Y    | 划账日期                                                     |
| abnormal_type      | short  | Y    | 异常类型：0 未知异常类型; 1 用户帐户异常; 2 平台异常; 3 清算渠道异常; |



### 1.7)请求返回结果示例

```
{
    "code": "1001",
    "seqno": "00588928118416202531675723189402",
    "data_node": "CN-South/DEV-1",
    "time_cost": 846,
    "payload": {
        "merchant": "62626601",
        "payee_name": "永泰县XX总公司",
        "payee_account": "1402080109814603875",
        "payee_type": "EnterpriseBankAccount",
        "bank_name": "中国工商银行",
        "bank_province": "上海市",
        "bank_city": "上海市",
        "bank_branch": "中国工商银行股份有限公司上海市永泰路支行",
        "id": "703279940799303681",
        "status": 0,
        "status_desc": "户名有误",
        "subject_template": "P云支付体验${day}日${type}",
        "bank_code": "102290006474",
        "withdraw_type": 0,
        "settle_cycle": "W1",
        "min_transfer_value": 5000,
        "priority": 5,
        "abnormal_type": 1,
        "merchant_name": "P云支付体验-停车场"
    }
}
```
