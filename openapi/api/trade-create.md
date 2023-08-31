# 原生交易请求

```
POST https://api.4pyun.com/gate/1.0/payment/trade/create
```

**接口说明**

	适用于APP/小程序通过原生发起交易，该交易方式需和平台方确认是否支持！

**请求参数**

| 字段名称     | 字段说明     |  类型  | 必填 | 示例  |
| :--- | :--- | :---: | :--: | :--- |
| app_id | 平台分配的接入应用ID     | string |  Y   | op1234567723122   |
| sub_app_id | 子应用ID: 微信小程序之类传入 | string | N | wx123232 |
| merchant     | 停车场商户号 | string |  Y   | 6262666666  |
| pay_order    | 发起支付订单号同一个app_id下唯一不能重复   | string |  Y   | PAYORDER-XXXXXXXX |
| subject      | 商品名称     | string |  Y   | 支付XX项目  |
| value  | 支付金额, 单位分   | string |  Y   | 100   |
| channel      | 支付渠道<br>普通微信支付宝被扫的时候不传其它类型类似银行被扫找研发同事提供, [参考附录定义](./../appendix.html) | string |  N   | 100001      |
| body   | 商品描述     | string |  N   | XXX停车场XXX车牌停车缴费XX元  |
| payer  | 付款方: 传入微信open_id小程序支付时必须传  | string |  N   | XXXXXXXXXXX |
| callback_url | 支付成功返回前端页面     | string |  N   | https://a.b.c/backurl   |
| auth_code    | 扣款授权码: 微信付款码/支付宝授权码(被扫必传    | string |  N   | 2012312313213123213     |
| notify_url   | 后端支付回调地址   | string |  Y   | https://a.b.c/notify    |
|expire_time|交易失效时间, 单位ms, 未设置默认3分钟失效, 格式: yyyy-MM-dd'T'HH:mm:ss'Z'|string|N|2021-09-02T09:36:46.020Z|
|trade_scene| 交易场景值, 根据商户交易按要求传递, 未按要求传递将无法正常支付, [参考附录定义](./../appendix.html) | string | Y | -|
| extra  | 根据交易场景传递, [参考附录定义](./../appendix.html)  | string |  N | {\"key1\":\"value1\",\"key2\":\"value2\"} |
| manual_settle | 仅聚合到账有效, 手动清算标记: 1-手动清算, 0-自动清算(默认)| int | N | 0 |
| sign   | 请求数据签名 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39    |


**请求示例**

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
  for (String key : map.keySet() {
      builder.append(key + "=" + map.get(key + "&");
  }
  String appSecret = "6409292d666251341234213";
  String encriptStr = builder.toString( + "app_secret=" + appSecret;
  System.out.println(encriptStr);
  String sign = MD5.encryptHEX(encriptStr);
  String keyStr = builder.toString( + "sign=" + sign;
  System.out.println(keyStr);
  try {
      Form form = Form.form();
      for (String key : map.keySet() {
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
  } catch (IOException e {
      e.printStackTrace();
  }
    }
```



**请求返回结果参数说明**

| 字段名称   | 字段说明     |  类型  | 必填 | 备注   |
| :--- | :--- | :---: | :--: | :--- |
| code | 请求状态码 | string |  Y | 1000-交易已受理(待支付)<br>1001-扣款成功<br>1403-支付订单号已存在<br>其它-读取message |
| message    | 返回描述     | string |  Y   | 返回描述     |
| hint | 返回错误说明 | string |  N   | 返回具体错描述指导 |
| seqno      | 服务器日志标示     | string |  Y   | 查日志用到查问题尽量提供这个值 |
| pay_serial | 支付平台订单号成功调用必返回   | string |  N   | 20210712215150075521111111     |


**请求返回结果示例:**

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
