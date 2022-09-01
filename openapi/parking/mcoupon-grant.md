# 停车优惠发放

### 1.1) 请求地址

	https://api.4pyun.com/gate/1.0/parking/mcoupon/grant/create

### 1.2) 调用方式

	HTTP POST FORM 表单提交

### 1.3) 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:merchant,store_code分别代表停车场商户号和商家商户号，两个参数由停车场物业那边提供
	3:coupon_code是具体一种优惠券的券ID，目前物业创建好优惠券充值好券之后截图然后找研发同事复制出来该值
	4:返回状态码code:1001代表派发成功，其它状态码直接读取message提示信息
	5:特别注意即使http状态非200也要把返回内容读取出来，会返回具体错误原因


### 1.4) 请求参数

| 字段名称       | 字段说明                               |  类型  | 必填 | 示例                             |
| :------------- | :------------------------------------- | :----: | :--: | :------------------------------- |
| app_id         | 平台分配的接入应用ID                   | string |  Y   | op1234567723122                  |
| sign           | 请求数据签名                           | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant       | 停车场商户号                           | string |  Y   | 6262666666                       |
| store_code     | 商家编号(停车场物业创建)               | string |  Y   | 6262666666                       |
| coupon_code    | 优惠券ID通过该字段指定需要派发的优惠券 | string |  Y   | CODE-XXXXXXXX                    |
| quantity       | 优惠张数                               | string |  Y   | 1                                |
| parking_serial | 停车场端的停车流水, 一般为入场记录ID   | string |  N   | PARKINGSERIAL-123456789          |
| plate          | 车牌号码                               | string |  Y   | 粤B12345                         |
| reason         | 派发原因说明                           | string |  N   | 商场消费                         |

### 1.5）请求示例
```
签名前字符串
1.1：签名前字符串
    str=app_id=op74061a7d244f7b7&coupon_code=7cb7cc3e4e27b8a&merchant=62626601&plate=粤BA1471&quantity=1&reason=测试派发&store_code=4911419421&app_secret=e9ae5402dbabc68c1c4xxxx
1.2：MD5(str)
    sign=756E3F88FBDB2B82E0A6C37C5EE060E5
   @Test
    public void testCouponGrant() {
        TreeMap<String, String> map = new TreeMap<>();
        // app_id应用身份ID
        map.put("app_id", "op74061a7d244f7b7");
        // 停车场商户号
        map.put("merchant", "62626601");
        // 商家商户号
        map.put("store_code", "4911419421");
        // 优惠券编码ID
        map.put("coupon_code", "7cb7cc3e4e27b8a");
        // 派发张数
        map.put("quantity", "1");
        // 派发车牌
        map.put("plate", "粤BA1471");
        // 派发原因说明
        map.put("reason","测试派发");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        // 和应用ID对应的应用加密密钥
        String appSecret = "e9ae5402dbabc68c1c4xxxx";
        String encriptStr = builder.toString() + "app_secret=" + appSecret;
        // 打印签名前字符串
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        String keyStr = builder.toString() + "sign=" + sign;
        System.out.println(keyStr);
        try {
            Form form = Form.form();
            for (String key : map.keySet()) {
                form.add(key, map.get(key));
            }
            // 添加签名字段
            form.add("sign", sign);
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/mcoupon/grant/create")
                    .bodyForm(form.build(), Charset.forName("utf-8"))
                    .execute();
            HttpResponse response1 = response.returnResponse();
            System.out.println(response1.getStatusLine());
            // 读取返回内容
            String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
            // 打印返回内容
            System.out.println(text);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```


### 1.6) 请求返回结果参数说明
| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                         |
| :------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:派发成功<br>400:参数错误<br>403:访问被拦截<br>500/1500:服务器内部错误<br>503:服务暂不可用 |
| message  | 返回描述       | string |  Y   | 返回描述                                                     |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |


### 1.7) 请求返回结果示例:

```
正常返回
{
  "code": "1001",
  "data_node": "HS1-1",
  "seqno": "86213123",
  "payload": {
    "grant_serial": "xxxxx"
  },
  "time_cost": 100,
  "message": "操作成功"
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
	"path": "POST /gate/1.0/parking/mcoupon/grant/create"
}
```

```
服务器内部错误
{
	"code": "1500",
	"message": "没有匹配到停车记录",
	"seqno": "57804ae2e859f58a",
	"data_node": "CN-South/HS3-1",
	"time_cost": 239,
	"payload": {
		"grant_serial": ""
	}
}
```
