# 交易预请求

```
POST https://api.4pyun.com/gate/1.0/payment/trade/prepare
```
**接口说明**

	1. app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2. 无感支付场景必须传递extra具体参考测试用例
	3. deduct_mode是无感状态同步接口返回的值，该值平台不关注具体值是多少,对接方也无需关心，微信或者支付宝透传给平台，平台再透传给调用方调用方再透传给到微信或者支付宝
	4. 该接口必须后端发起请求不能直接在前端调用该接口
	5. `callback_url`在小程序调用场景，仅支持当前小程序内页面跳转，需传入例如`/pages/index/index`！

**请求参数**

| 字段名称 | 字段说明 |  类型  | 必填 | 示例  |
| :--- | :--- | :---: | :--: | :--- |
| app_id | 平台分配的接入应用ID | string |  Y | op1234567723122 |
| merchant | 停车场商户号 | string |  Y | 6262666666  |
| pay_order  | 发起支付订单号同一个app_id下唯一不能重复 | string |  Y | PAYORDER-XXXXXXXX |
| subject  | 商品名称 | string |  Y | 支付XX项目  |
| value  | 支付金额, 单位分 | string |  Y | 100 |
| body | 商品描述 | string |  N | XXX停车场XXX车牌停车缴费XX元  |
| payer  | 付款方: 传入微信/支付宝/其他openid | string |  N | XXXXXXXXXXX |
| deduct_mode  | 扣款模式: 车主服务传入(透传无感同步接口4.4返回的值  | string |  N | PROACTIVE |
| callback_url | 支付成功返回前端页面 | string |  N | https://a.b.c/backurl |
| notify_url | 后端支付回调地址 | string |  Y | https://a.b.c/notify  |
|expire_time|交易失效时间, 未设置默认3分钟失效, 格式: yyyy-MM-dd'T'HH:mm:ss'Z' <br>特别说明UTC时间和普通时间差8小时因为我们在东八区 2022-09-01T00:00:00.000Z 对应时间的时间是 2022-09-01 08:00:00|string|N|2021-09-02T09:36:46.020Z|
|trade_scene| 交易场景值, 根据商户交易按要求传递, 未按要求传递将无法正常支付 | string | Y | -|
| extra  | 根据支付场景值传递  | string |  N | {\"key1\":\"value1\",\"key2\":\"value2\"} |
| sign | 请求数据签名 | string |  Y | C65FCAC2D3FB5E2D3D4AD93DD20C8C39  |

**支付场景**

|场景值|说明|
|---|---|
|PARKING|停车缴费(临停续费)|
|ENERGY|充电业务|

**业务参数**
```
PARKING 场景填写到extra字段
```
| 字段名称 | 字段说明 |  类型  | 必填 | 示例  |
| :--- | --- | :---: | :--: | :--- |
| plate  | 支付车牌 | string |  Y | 粤TXXXXX  |
| plate_color  | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, <br>4.黑色, 5.绿色, -1 未知 | string |  Y | 1 |
| card_id  | 停车卡ID, 无车牌可传递 | string |  N | ABDDS  |
| gate_id  | 缴费通道ID | string |  N | 001  |
| parking_serial | 一次停车唯一ID | string |  Y | XXXXX-ID  |
| park_name  | 停车场名称 | string |  Y | XXXX-停车场 |
| enter_time | 入场时间, 单位毫秒 | string |  Y | 1552976318722 |
| parking_time | 停车时长, 单位秒 | string |  Y | 3600  |
| free_value | 车场优惠金额, 单位分 | int |  Y | 100  |
| receipt | 微信点金计划页面显示按钮为`我要开票`, 1 显示, 其他无效 | string |  N | 1  |


**请求示例**

```
签名前字符串
1.1：签名前字符串
  str=app_id=op0096196358123123&body=【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费1.00元&callback_url=https://a.b.c/backurl&channel=200201&extra={"plate_color":"-1","parking_serial":"01009970545516206225805859659","plate":"川AA37K2","enter_time":"1620621689000","park_name":"P云支付体验-停车场","parking_time":"175"}&merchant=62626601&notify_url=https://a.b.c/notify&pay_order=11111111118&subject=停车支付3.00元(川A12345)&value=10&app_secret=6409292d66625a2a12342134
1.2：MD5(str)
  sign=96FD052F81BBB3C25B5BD628E189A154
		@Test
  public void testPaymentPrepare(){
  TreeMap<String, String> map = new TreeMap<>();
  // app_id平台分配
  map.put("app_id", "op0096196358123123");
  // 停车场商户号
  map.put("merchant", "62626601");
  // 发起支付订单号同一个app_id下唯一不能重复
  map.put("pay_order", "11111111118");
  // 商品名称
  map.put("subject","停车支付3.00元(川A12345)");
  // 商品描述
  map.put("body","【川A12345】在【XXXXX-停车场】于【2021-07-01 00:00:00】支付费1.00元");
  // 支付金额, 单位分
  map.put("value","10");
  // 支付渠道 微信传200201 支付宝传100102
  map.put("channel","200201");
  // 支付成功返回前端页面
  map.put("callback_url", "https://a.b.c/backurl");
  // 后端支付回调地址
  map.put("notify_url", "https://a.b.c/notify");
  Map<String, String> extraMap = new HashMap<>();
  extraMap.put("plate", "川AA37K2");
  extraMap.put("plate_color", "-1");
  extraMap.put("parking_serial", "01009970545516206225805859659");
  extraMap.put("park_name", "P云支付体验-停车场");
  extraMap.put("enter_time", "1620621689000");
  extraMap.put("parking_time", "175");
  map.put("extra", JSON.toJSONString(extraMap));
  StringBuilder builder = new StringBuilder();
  for (String key : map.keySet( {
  builder.append(key + "=" + map.get(key + "&");
  }
  String appSecret = "6409292d66625a2a12342134";
  String encriptStr = builder.toString( + "app_secret=" + appSecret;
  System.out.println(encriptStr);
  String sign = MD5.encryptHEX(encriptStr);
  String keyStr = builder.toString( + "sign=" + sign;
  System.out.println(keyStr);
  try {
  Form form = Form.form();
  for (String key : map.keySet( {
  form.add(key, map.get(key));
  }
  form.add("sign", sign);
  Response response = Request.Post("https://api.4pyun.com/gate/1.0/payment/trade/prepare")
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

| 字段名称 | 字段说明 |  类型  | 必填 | 备注  |
| :--- | :--- | :---: | :--: | :--- |
| code | 请求状态码 | string |  Y | 1001:扣款成功<br>400:参数错误<br>其它状态码:读取message |
| message  | 返回描述 | string |  N | 返回描述  |
| hint | 返回错误说明 | string |  N | 返回具体错描述指导  |
| seqno  | 服务器日志标示 | string |  Y | 查日志用到查问题尽量提供这个值  |
| pay_url  | 可支付链接成功状态必返回 | string |  N | https://app.4pyun.com/payment/trade/create?pay_id=XXXXX |
| pay_id | 支付平台订单ID成功状态必返回 | string |  N | 20210712215150075521111111  |


**请求返回结果示例:**


```
成功返回
{
	"code": "1001",
	"seqno": "92a78431551bd293",
	"data_node": "CN-South/HS3-1",
	"pay_url": "https://app.4pyun.com/payment/trade/create?pay_id=621722601479223111111111",
	"pay_id": "621722601479223111111111"
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
	"path": "POST /gate/1.0/payment/trade/prepare"
}
```

```
扣款错误
{
	"code": "500",
	"message": "订单号已存在",
	"seqno": "93128ddd750dc90e",
	"data_node": "CN-South/HS3-3",
	"path": "POST /gate/1.0/payment/trade/prepare"
}
```
