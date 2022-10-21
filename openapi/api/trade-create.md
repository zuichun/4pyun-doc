# 被扫发起交易请求

### 1.1) 请求地址

	https://api.4pyun.com/gate/1.0/payment/trade/create

### 1.2) 调用方式

	HTTP POST FORM 表单提交

### 1.3) 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:如果是微信支付宝被扫必须传auth_code注意这种情况下不传channel平台自动识别微信支付宝，如果是银行相关被扫主动联系研发确认具体银行的被扫channel值需要调用方那边配置写死
	3:小程序支付必须传sub_app_id(小程序app_id)和payer(小程序用户open_id)
	4:deduct_mode是无感状态同步接口返回的值，该值平台不关注具体值是多少,对接方也无需关心，微信或者支付宝透传给平台，平台再透传给调用方调用方再透传给到微信或者支付宝



### 1.4) 请求参数

| 字段名称     | 字段说明                                                     |  类型  | 必填 | 示例                                      |
| :----------- | :----------------------------------------------------------- | :----: | :--: | :---------------------------------------- |
| app_id       | 平台分配的接入应用ID                                         | string |  Y   | op1234567723122                           |
| sub_app_id   | 子应用ID: 例如微信小程序app_id                               | string |  N   | op1231231323213                           |
| merchant     | 停车场商户号                                                 | string |  Y   | 6262666666                                |
| pay_order    | 发起支付订单号同一个app_id下唯一不能重复                     | string |  Y   | PAYORDER-XXXXXXXX                         |
| subject      | 商品名称                                                     | string |  Y   | 支付XX项目                                |
| value        | 支付金额, 单位分                                             | string |  Y   | 100                                       |
| channel      | 支付渠道<br>被扫的时候不传<br>微信传200201<br>支付宝传100102<br>其它找研发同事提供 | string |  N   | 100001                                    |
| body         | 商品描述                                                     | string |  N   | XXX停车场XXX车牌停车缴费XX元              |
| payer        | 付款方: 传入微信open_id小程序支付时必须传                    | string |  N   | XXXXXXXXXXX                               |
| callback_url | 支付成功返回前端页面                                         | string |  N   | https://a.b.c/backurl                     |
| auth_code    | 扣款授权码: 微信付款码/支付宝授权码(被扫必传)                | string |  N   | 2012312313213123213                       |
| notify_url   | 后端支付回调地址                                             | string |  Y   | https://a.b.c/notify                      |
|expire_time|交易失效时间, 单位ms, 未设置默认3分钟失效, 格式: yyyy-MM-dd'T'HH:mm:ss'Z'|string|N|2021-09-02T09:36:46.020Z|
| extra        | 额外附加参数Map结构(微信无感)                                | string |  N   | {\"key1\":\"value1\",\"key2\":\"value2\"} |
| sign         | 请求数据签名                                                 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39          |




### 1.5）请求示例

```
被扫示例
签名前字符串
1.1：签名前字符串
    str=app_id=op009619631234213&auth_code=135680596336872323&body=【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费1.00元&merchant=62626601&notify_url=https://a.b.c/notify&pay_order=11111111111&payer=o1kTSt3wG76-vZh_ORgd9Ef6MXYQ&subject=停车支付3.00元(川A12345)&value=10&app_secret=6409292d666251341234213
1.2：MD5(str)
    sign=8E0050337ACD79D9FFA44CFCFD6A2919
		@Test
    public void testPaymentCreateAuthCode(){
        TreeMap<String, String> map = new TreeMap<>();
        // app_id平台分配
        map.put("app_id", "op009619631234213");
        // 停车场商户号
        map.put("merchant", "62626601");
        // 发起支付订单号同一个app_id下唯一不能重复
        map.put("pay_order", "11111111111");
        // 商品名称
        map.put("subject","停车支付3.00元(川A12345)");
        // 商品描述
        map.put("body","【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费0.10元");
        // 支付金额, 单位分
        map.put("value","10");
        // 扫码得到的值
        map.put("auth_code", "135680596336872323");
        // 后端支付回调地址
        map.put("notify_url", "https://a.b.c/notify");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "6409292d666251341234213";
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
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/payment/trade/create")
                    .bodyForm(form.build(), Charset.forName("utf-8"))
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

```
小程序示例
1.1：签名前字符串
    str=app_id=op009619635812342&body=【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费1.00元&callback_url=https://a.b.c/backurl&channel=200201&merchant=62626601&notify_url=https://a.b.c/notify&pay_order=11111111117&payer=o1kTSt3wG76-vZh_ORgd9Ef6MXYQ&subject=停车支付3.00元(川A12345)&value=10&app_secret=6409292d66625a2a0912acfc611234
1.2：MD5(str)
    sign=464E5E893B05657E653D0BA8E3F3C9F3
 		@Test
    public void testPaymentCreateXiaoXu(){
        TreeMap<String, String> map = new TreeMap<>();
        // app_id平台分配
        map.put("app_id", "op009619635811234");
        // 小程序app_id
        map.put("app_id", "op009619635812342");
        // 停车场商户号
        map.put("merchant", "62626601");
        // 发起支付订单号同一个app_id下唯一不能重复
        map.put("pay_order", "11111111117");
        // 商品名称
        map.put("subject","停车支付3.00元(川A12345)");
        // 商品描述
        map.put("body","【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费1.00元");
        // 支付金额, 单位分
        map.put("value","10");
        // 支付渠道 微信传200201 支付宝传100102
        map.put("channel","200201");
        // 付款人open_id
        map.put("payer","o1kTSt3wG76-vZh_ORgd9Ef6MXYQ");
        // 支付成功返回前端页面
        map.put("callback_url", "https://a.b.c/backurl");
        // 后端支付回调地址
        map.put("notify_url", "https://a.b.c/notify");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "6409292d66625a2a0912acfc611234";
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
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/payment/trade/create")
                    .bodyForm(form.build(), Charset.forName("utf-8"))
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


### 1.6) 请求返回结果参数说明
| 字段名称   | 字段说明                                                     |  类型  | 必填 | 备注                                                         |
| :--------- | :----------------------------------------------------------- | :----: | :--: | :----------------------------------------------------------- |
| code       | 请求状态码                                                   | string |  Y   | 1000:扣款受理成功<br>1001:扣款成功<br>400:参数错误<br>其它状态码:读取message |
| message    | 返回描述                                                     | string |  Y   | 返回描述                                                     |
| hint       | 返回错误说明                                                 | string |  N   | 返回具体错描述指导                                           |
| seqno      | 服务器日志标示                                               | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| trade      | 小程序支付原生微信返回<br>具体参考微信小程序支付<br>字段意义 | string |  N   | {<br/>	"appId": "wx27658dfbXXXXXX",<br/>	"timeStamp": "1614394265",<br/>	"nonceStr": "ehs1YYDw5abhs5xVk805UetnU8BGdPP2",<br/>	"signType": "MD5",<br/>	"paySign": "995C8C98AA49AF07B16B9C7B2BA51411",<br/>	"package": "prepay_id=wx27105105573267eXXXXXXX20fb0000"<br>} |
| pay_serial | 支付平台订单号成功调用必返回                                 | string |  N   | 20210712215150075521111111                                   |


### 1.7) 请求返回结果示例:

```
被扫正常返回扣款已经受理
{
	"code": "1000",
	"seqno": "9ce2f9ac8816496d",
	"data_node": "CN-South/HS3-2",
	"trade": "",
	"pay_serial": "20210712215150075521111111"
}
```

```
被扫扣款成功
{
	"code": "1001",
	"seqno": "9ce2f9ac8816496d",
	"data_node": "CN-South/HS3-2",
	"trade": "",
	"pay_serial": "20210712215150075521111111"
}
```

```
参数错误
{
	"code": "400",
	"message": "请求参数错误",
	"hint": "`merchant` Required!",
	"seqno": "94929a9b0874aa46",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/payment/trade/create"
}
```

```
扣款错误
{
	"code": "500",
	"message": "订单号已存在",
	"seqno": "93128ddd750dc90e",
	"data_node": "CN-South/HS3-3",
	"path": "POST /gate/1.0/payment/trade/create"
}
```
