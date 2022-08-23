# 3 查询交易状态

### 3.1) 请求地址

	https://api.4pyun.com/gate/1.0/payment/trade/query

### 3.2) 请求方式

	HTTP GET

### 3.3) 特殊说明

	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:pay_serial,pay_order两个参数至少传一个

### 3.4) 请求参数

| 字段名称     | 字段说明                                                     |  类型  | 必填 | 示例                                      |
| :----------- | :----------------------------------------------------------- | :----: | :--: | :---------------------------------------- |
| app_id       | 平台分配的接入应用ID                                         | string |  Y   | op1234567723122                           |
| merchant     | 停车场商户号                                                 | string |  Y   | 6262666666                                |
| pay_order    | 支付订单号                                                   | string |  N   | PAYORDER-XXXXXXXX                         |
| pay_serial   | 平台支付流水                                                 | string |  N   | PAYSERIAL-XXXXXXXX                        |
| sign         | 请求数据签名                                                 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39          |

### 3.5）请求示例

```
签名前字符串
1.1：签名前字符串
str=app_id=op009619634544&merchant=62626666666&pay_order=00202101010000060755912770312323&app_secret=6409292d66625a2a0912ac345345
1.2：MD5(str)
    sign=8FFDCC0A8C51BB92F183CAE2DD0C70D6
    @Test
    public void testPaymentQuery1(){
        TreeMap<String, String> map = new TreeMap<>();
        // app_id平台分配
        map.put("app_id", "op009619634544");
        // 停车场商户号
        map.put("merchant", "62626666666");
        // 发起支付订单号同一个app_id下唯一不能重复
        map.put("pay_order", "00202101010000060755912770312323");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "6409292d66625a2a0912ac345345";
        String encriptStr = builder.toString() + "app_secret=" + appSecret;
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        String keyStr = builder.toString() + "sign=" + sign;
        System.out.println(keyStr);
        try {
            Form form = Form.form();
            for (String key : map.keySet()) {
                form.add(key, map.get(key));
            }
            form.add("sign", sign);
            Response response = Request.Get("https://api.4pyun.com/gate/1.0/payment/trade/query?"+keyStr)
                    .execute();
            HttpResponse response1 = response.returnResponse();
            System.out.println(response1.getStatusLine());
            String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
            System.out.println(text);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```


### 3.6) 请求返回结果参数说明
| 字段名称   | 字段说明                                           |  类型  | 必填 | 备注                                                    |
| :--------- | :------------------------------------------------- | :----: | :--: | :------------------------------------------------------ |
| code       | 请求状态码                                         | string |  Y   | 1001:查询成功(不代表支付结果)<br>400:参数错误<br>其它状态码:读取message |
| message    | 返回描述                                           | string |  N   | 返回描述                                                |
| hint       | 返回错误说明                                       | string |  N   | 返回具体错描述指导                                      |
| seqno      | 服务器日志标示                                     | string |  Y   | 查日志用到查问题尽量提供这个值                          |
| merchant   | 停车场商户号                                       | string |  Y   | 6262666666                                              |
| pay_order  | 支付订单号                                         | string |  N   | 20210712215150075521111111                              |
| channel    | 微信:200201<br>支付宝:100102<br>其它找研发同事提供 | string |  N   | 100001                                                  |
| pay_serial | 平台支付流水                                       | string |  N   | PAYSERIAL-XXXXXXXX                                      |
| value      | 支付金额, 单位分                                   | string |  N   | 100                                                     |
| status     | 交易状态: 1支付成功、-1失败、0支付中               | string |  N   | 1                                                       |
| trade_time | 支付完成时间                                       | string |  N   | 2020-12-31T16:00:15Z                                    |
| payer      | 微信/支付宝/其他openid                             | string |  N   | XXXXXXXXXXX                                             |


### 3.7) 请求返回结果示例:


```
成功返回
{
	"code": "1001",
	"seqno": "8eb5e612ecea0fbd",
	"data_node": "CN-South/HS3-2",
	"merchant": "6262666666666",
	"pay_order": "00202101010000021312323",
	"channel": "200201",
	"pay_serial": "20210101000009123423423",
	"value": 500,
	"status": 1,
	"trade_time": "2020-12-31T16:00:15Z",
	"payer": "o1kTSt6i42h40c-B1NZZ4u-23434"
}
```

```
{
	"code": "400",
	"message": "请求参数错误",
	"hint": "参数`merchant`未传递",
	"seqno": "aef05d52317e0366",
	"data_node": "CN-South/HS3-2",
	"path": "GET /gate/1.0/payment/trade/query"
}
```

```
查询缺少必要参数
{
	"code": "500",
	"message": "处理异常:java.lang.IllegalArgumentException: `pay_serial`/`merchant`+`order` Required!",
	"seqno": "e20dd263e302ddac",
	"data_node": "CN-South/HS3-3",
	"path": "GET /gate/1.0/payment/trade/query"
}
```

```
查询失败
{
	"code": "500",
	"message": "未查询到订单记录",
	"seqno": "1c05072f5f026468",
	"data_node": "CN-South/HS3-3",
	"path": "GET /gate/1.0/payment/trade/query"
}
```
