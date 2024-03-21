# 发起订单退款


### 4.1) 请求地址

	https://api.4pyun.com/gate/1.0/payment/trade/refund

### 4.2) 调用方式

	HTTP POST application/json

### 4.3) 特殊说明

	1:app_id,app_secret 应用id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:该退款接口只允许对已授权商户的订单发起退款，请联系研发进行授权配置

### 4.4) 请求参数

| 字段名称   | 字段说明                                  |  类型  | 必填 | 示例                       |
| :--------- | :---------------------------------------- | :----: | :--: | :------------------------- |
| app_id     | 平台分配的接入应用ID                      | string |  Y   | op1234567723122            |
| order | 退款请求单号                              | string |  N   | 20220217161036075521110362 |
| pay_serial | 平台支付流水                              | string |  Y   | 20220217161036075521110362 |
| value      | 退款金额(单位分，大于0, 小于等于订单金额) | string |  Y   | 1                          |
| reason     | 退款原因                                  | string |  N   | 客户要求退款               |



### 4.5）请求示例

```
签名前字符串
1.1：签名前字符串（Json请求报文拼接“&app_secret=${app_secret}”）
    {"reason":"接口测试退款","pay_serial":"20220721102644066066610031","app_id":"op00961963581daa7","value":"1"}&app_secret=6409292d66625a2a0912acfc61ed956c
1.2：MD5(str)
    sign=55D9BC675B3B042A015895FA9F9D037B

    @Test
    public void testRefundCreate() {
        String url = "https://dev-api.4pyun.com/gate/1.0/payment/refund/create";
        String appId = "op00961963581daa7";
        String appSecret = "6409292d66625a2a0912acfc61ed956c";

        Map<String, String> paramMap = Maps.newHashMap();
        // 应用ID
        paramMap.put("app_id", appId);
	// 退款请求单号
        paramMap.put("order", "R2024032114351106991");
        // 平台支付订单号
        paramMap.put("pay_serial", "20220721143511066066610042");
        // 退款金额
        paramMap.put("value", "1");
        // 退款原因
        paramMap.put("reason", "接口测试退款");
        // 转成Json参数格式
        String jsonStr = JSON.toJSONString(paramMap);
        // 生成待签名参数
        String encryptStr = jsonStr + "&app_secret=" + appSecret;
        //
        System.out.println("encryptStr: " + encryptStr);
        String sign = MD5.encryptHEX(encryptStr);
        System.out.println("sign: " + sign);

        try {
            System.out.println("Request: " + jsonStr);
            Response response = Request.Post(url)
                    // 签名设置在请求头，header-name为"Authorization"
                    .setHeader("Authorization", sign)
                    // 请求参数格式为application/json;charset=utf-8
                    .bodyString(jsonStr, ContentType.APPLICATION_JSON.withCharset(CharsetUtils.UTF_8))
                    .execute();
            String responseBody = EntityUtils.toString(response.returnResponse().getEntity());
            System.out.println("Response: " + responseBody);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```


### 4.6) 请求返回结果参数说明
| 字段名称     | 字段说明       |  类型  | 必填 | 备注                                                         |
| :----------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code         | 请求状态码     | string |  Y   | 1001:退款成功<br>400:参数错误<br>1403:没有该订单退款权限<br>1003:可用退款余额不足<br>1405:退款失败<br>其它状态码:读取message |
| message      | 返回描述       | string |  N   | 返回描述                                                     |
| hint         | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno        | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| pay_serial   | 平台支付流水   | string |  N   | 20220719163604066066610014                                   |
| refund_order  | 平台退款订单号 | string |  N   | 20220725004608075527054                                      |
| refund_serial | 第三方退款流水 | String |  N   | 50301802862022072523029437751                                |
| refund_time   | 退款时间       | String |  N   | 2022-02-17T08:12:46.616Z                                     |


### 4.7) 请求返回结果示例:


```
成功返回
{
    "code": "1001",
    "seqno": "9937485877757468",
    "data_node": "CN-South/DEV-1",
    "time_cost": 2854,
    "payload": {
        "pay_serial": "20220719163604066066610014",
        "trade": "",
        "refund_order": "20220725004608075527054",
        "refund_serial": "50301802862022072523029437751",
        "refund_time": "2022-07-24T16:46:09.755Z",
        "message": "",
        "extra": {}
    }
}
```

```
参数错误
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "`pay_serial` Required!",
    "seqno": "1056831907048775",
    "data_node": "CN-South/DEV-1",
    "path": "POST /gate/1.0/payment/refund/create"
}
```

```json
退款失败
{
    "code": "1405",
    "message": "[INVALID_REQUEST]订单已全额退款",
    "seqno": "8796312212702133",
    "data_node": "CN-South/DEV-1",
    "time_cost": 906,
    "payload": {
        "pay_serial": "",
        "trade": "",
        "refund_order": "",
        "refund_serial": "",
        "message": "",
        "extra": {}
    }
}
```

```
没有该订单退款权限
{
    "code": "1403",
    "message": "没有该订单退款权限",
    "seqno": "1660906837934964",
    "data_node": "CN-South/DEV-1",
    "time_cost": 17,
    "payload": {
        "pay_serial": "",
        "trade": "",
        "refund_order": "",
        "refund_serial": "",
        "message": "",
        "extra": {}
    }
}
```

```
可用退款余额不足
{
    "code": "1003",
    "message": "可用退款余额不足, 退款金额=0.01元, 可用余额=0.0元",
    "seqno": "6714728479127389",
    "data_node": "CN-South/DEV-1",
    "time_cost": 55,
    "payload": {
        "pay_serial": "",
        "trade": "",
        "refund_order": "",
        "refund_serial": "",
        "message": "",
        "extra": {}
    }
}
```
