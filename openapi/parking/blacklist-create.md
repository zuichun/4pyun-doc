# 黑名单车辆添加

### 1.1) 请求地址

 https://api.4pyun.com/gate/1.0/parking/parking/blacklist

### 1.2) 调用方式

 HTTP POST FORM 表单提交

### 1.3) 特殊说明

 1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请


### 1.4) 请求参数

| 字段名称   | 字段说明             |  类型  | 必填 | 示例                             |
| :--------- | :------------------- | :----: | :--: | :------------------------------- |
| app_id     | 平台分配的接入应用ID | string |  Y   | op1234567723122                  |
| sign       | 请求数据签名         | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant   | 停车场商户号         | string |  Y   | 6262666666                       |
| plate      | 车牌号码             | string |  Y   | 粤TXXXXX                         |
| reason     | 添加原因             | string |  Y   | 逃费                             |
| start_time | 生效时间             | string |  Y   | 2020-12-31T16:00:15Z             |
| end_time   | 到期时间             | string |  Y   | 2020-12-31T16:00:15Z             |


### 1.5) 请求返回结果参数说明

| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                |
| :------- | :------------- | :----: | :--: | :-------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:添加成功<br>400:参数错误<br>500:服务器内部错误 |
| message  | 返回描述       | string |  N   | 返回描述                                            |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                  |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                      |


### 1.6) 请求返回结果示例:

```
正常返回
{
 "code": "1001",
 "seqno": "2fe5f0a68b4ae19a4be8c32a1d73a6e7",
 "data_node": "CN-South/HS3-3",
 "time_cost": 88
}
```
