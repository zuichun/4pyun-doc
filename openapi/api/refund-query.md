# 退款订单查询


### 4.1) 请求地址

	https://api.4pyun.com/gate/1.0/payment/trade/refund

### 4.2) 调用方式

	HTTP GET

### 4.3) 特殊说明

	1:app_id,app_secret 应用id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:该退款查询接口只允许查询已授权商户的订单，请联系研发进行授权配置

### 4.4) 请求参数

| 字段名称 | 字段说明                             |  类型  | 必填 | 示例                       |
| :------- | :----------------------------------- | :----: | :--: | :------------------------- |
| app_id   | 平台分配的接入应用ID                 | string |  Y   | op1234567723122            |
| merchant | 退款订单所属商户号                   | string |  Y   | 62626601                   |
| order    | 退款请求单号，对应发起退款时传入参数 | string |  Y   | 20220217161036075521110362 |



### 4.5）请求示例

```java
签名算法：
1. 设所有发送或接收到的数据为集合M，将集合M内非空参数值的参数按照参数名 ASCII码 从小到大排序（字典序），使用 URL键值对 的格式（即 key1=value1&key2=value2...）拼接成字符串 stringA。
2. sign = stringA + "&app_secret=" + appSecret，取 MD5（32位不区分大小写）。
  
注意事项：
1. 参数名 ASCII码 从小到大排序（字典序）；
2. 如果参数值为空（即null）不参与签名；
3. 参数名区分大小写；
4. 验证签名时，sign 参数不参与签名，将生成的签名与该 sign 值作校验；
5. 接口可能增加字段，验证签名时必须支持增加的扩展字段；

    @Test
    public void testRefundQuery() {
        String url = "https://dev-api.4pyun.com/gate/1.0/payment/refund/create";
        String appId = "op00961963581daa7";
        String appSecret = "6409292d66625a2a0912acfc61ed956c";

		Map<String, String> paramMap = Maps.newTreeMap();
		// 应用ID
        paramMap.put("app_id", appId);
		// 退款订单所属商户号
        paramMap.put("merchant", "62626601");
		// 退款请求单号
        paramMap.put("order", "TEST_20240320145031953");
        
		// 生成待签名参数
        String str = StringUtils.join(paramMap, "&", "=") + "&app_secret=" + appSecret;
        System.out.println("encryptStr: " + str);
  
		// 生成签名
        String sign = MD5.encryptHEX(str);
        System.out.println("sign: " + sign);
        paramMap.put("sign", sign);
  
		// 组装请求URL
        String requestUrl = UrlUtils.build(url, paramMap);

        try {
            System.out.println("Request: " + requestUrl);
            HttpResponse httpResponse = Request.Get(requestUrl)
                    .execute()
                    .returnResponse();
            String responseBody = EntityUtils.toString(httpResponse.getEntity());
            System.out.println("Response: " + responseBody);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```


### 4.6) 请求返回结果参数说明
| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                         |
| -------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:查询成功<br>1002:退款订单不存在<br/>1400:参数错误<br>1403:应用权限错误<br>其它状态码:读取message |
| message  | 返回描述       | string |  N   | 返回描述                                                     |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| payload  | 退款订单信息   |  json  |  N   |                                                              |

| payload字段名称 | 字段说明 | 类型 | 必填 | 备注 |
| --------------- | -------- | ---- | ---- | ---- |
| merchant        | 商户号 | string | Y |      |
| merchant_name   | 商户名称 | string | Y |      |
| order           | 退款请求单号 | string | Y |  |
| refund_order    | 退款订单号 | string | Y |      |
| refund_serial   | 退款流水 | string | N |      |
| reason   | 退款原因 | string | N |      |
| receipt_url   | 退款凭证 | string | N |      |
| pay_serial      | 原支付流水 | string | Y |      |
| trade_subject      | 原交易主题 | string | Y |      |
| trade_body      | 原交易内容 | string | Y |      |
| value           | 退款金额(单位分) | int | Y |      |
| process         | 退款进度: 0 退款中, 1 退款成功, -1 退款失败 | string | Y |      |
| create_time     | 创建时间 | string | Y |      |
| refund_time     | 退款时间 | string | N | 格式如：2022-02-17T08:12:46.616Z |
| operator_id     | 退款操作员ID | string | Y |      |
| operator_name   | 退款操作员名称 | string | Y |      |



### 4.7) 请求返回结果示例:


```json
查询成功
{
    "code": "1001",
    "seqno": "60661674882214038340794721836706",
    "data_node": "CN-South/DEV-3",
    "time_cost": 14,
    "payload": {
        "order": "TEST_20240321165705440",
        "refund_order": "20240321165706075524320",
        "refund_serial": "1770736381283725312",
        "pay_serial": "20240321165625066020110009",
        "value": 1,
        "process": 1,
        "create_time": "2024-03-21T08:57:06Z",
        "refund_time": "2024-03-21T08:57:08Z",
        "operator_id": "op94450a8603f8843",
        "operator_name": "测试应用",
        "reason": "测试退款",
        "merchant": "97919537",
        "merchant_name": "测试进件240206-1-停车场",
        "trade_subject": "停车支付0.01元(粤BXXXXX)",
        "trade_body": "【粤BXXXXX】在【测试进件240206-1-停车场】支付停车费0.01元",
        "receipt_url": ""
    }
}
```

```json
参数错误
{
    "code": "1400",
    "message": "`order` is required",
    "seqno": "28610145597796892868994206094424",
    "data_node": "CN-South/DEV-3",
    "time_cost": 116,
    "payload": {
        "order": "",
        "refund_order": "",
        "refund_serial": "",
        "pay_serial": "",
        "value": 0,
        "process": 0,
        "operator_id": "",
        "operator_name": "",
        "reason": "",
        "merchant": "",
        "merchant_name": "",
        "trade_subject": "",
        "trade_body": "",
        "receipt_url": ""
    }
}
```

```json
退款订单不存在
{
    "code": "1002",
    "message": "退款订单不存在",
    "hint": "merchant=97919537, order=TEST_20240321165705441",
    "seqno": "41233945733378583301176401889043",
    "data_node": "CN-South/DEV-3",
    "time_cost": 6,
    "payload": {
        "order": "",
        "refund_order": "",
        "refund_serial": "",
        "pay_serial": "",
        "value": 0,
        "process": 0,
        "operator_id": "",
        "operator_name": "",
        "reason": "",
        "merchant": "",
        "merchant_name": "",
        "trade_subject": "",
        "trade_body": "",
        "receipt_url": ""
    }
}
```

```json
没有该订单退款权限
{
    "code": "1403",
    "message": "该商户未授权该应用",
    "hint": "app_id=op94450a8603f8843, merchant=17627004",
    "seqno": "91250735701538035211017554555339",
    "data_node": "CN-South/DEV-3",
    "time_cost": 603,
    "payload": {
        "order": "",
        "refund_order": "",
        "refund_serial": "",
        "pay_serial": "",
        "value": 0,
        "process": 0,
        "operator_id": "",
        "operator_name": "",
        "reason": "",
        "merchant": "",
        "merchant_name": "",
        "trade_subject": "",
        "trade_body": "",
        "receipt_url": ""
    }
}
```

