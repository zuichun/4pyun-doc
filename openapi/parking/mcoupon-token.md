# 停车优惠券二维码

### 2.1) 请求地址

	https://api.4pyun.com/gate/1.0/parking/mcoupon/token

### 2.2) 调用方式

	HTTP POST FORM 表单提交

### 2.3) 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:store_code由停车场创建商家商家列表里面的商家号就是store_code
	3:coupon_code是具体一种优惠券的券ID，目前物业创建好优惠券充值好券之后截图然后找研发同事复制出来该值
	4:返回状态码code:1001代表派发成功，其它状态码直接读取message提示信息
	5:use_expire 传1代表有一个人领取了优惠二维码就失效，不传或者传0判断5分钟时长和expire_in取最大值，也就是二维码的最短有效时长是5分钟，最长有效时间是expire_in


### 2.4) 请求参数

| 字段名称    | 字段说明                                    |  类型  | 必填 | 示例                             |
| :---------- | :------------------------------------------ | :----: | :--: | :------------------------------- |
| app_id      | 平台分配的接入应用ID                        | string |  Y   | op1234567723122                  |
| sign        | 请求数据签名                                | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| store_code  | 商家编号(停车场物业创建)                    | string |  Y   | 二进制文件流                     |
| coupon_code | 优惠券ID通过该字段指定需要派发的优惠券      | string |  Y   | CODE-XXXXXXXX                    |
| quantity    | 优惠张数                                    | string |  Y   | 1                                |
| use_expire  | 领券之后优惠码是否立马失效1:代表是，0代表否 | string |  N   | 1                                |
| expire_in   | 优惠码生成之后多久之后失效 单位秒默认5分钟  | string |  N   | 300                              |
| reason      | 派发原因说明                                | string |  N   | 商场消费                         |

### 2.5）请求示例
```
签名前字符串
1.1：签名前字符串    str=app_id=op2728846bdb3780e&coupon_code=7cb7cc3e4e27b8a&expire_in=600&quantity=1&store_code=92908284598&use_expire=1&app_secret=ags3j97jdnbcizb5z8ubc1xijc011zcj
1.2：MD5(str)
    sign=0BB9C86B96957D60B4A266FC9CEF1965
    @Test
    public void tokenCreate1() {
        TreeMap<String, String> map = new TreeMap<>();
        // app_id应用身份ID
        map.put("app_id", "op2728846bdb37123");
        // 商家商户号
        map.put("store_code", "92908284598");
        // 优惠券编码ID
        map.put("coupon_code", "7cb7cc3e4e27b8a");
        // 派发张数
        map.put("quantity", "1");
        // 领取优惠之后优惠券是否失效 1：领取后立马失效
        map.put("use_expire", "1");
        // 指定多久后失效, 单位秒,不传默认5分钟，传了小于5分钟不生效大于5分钟才生效
        map.put("expire_in", 60 * 10 + "");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "ags3j97jdnbcizb5z8ubc1xijc23123";
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
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/mcoupon/token")
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


### 2.6) 请求返回结果参数说明
| 字段名称  | 字段说明                 |  类型  | 必填 | 备注                                                         |
| :-------- | :----------------------- | :----: | :--: | :----------------------------------------------------------- |
| code      | 请求状态码               | string |  Y   | 1001:派发成功<br>400:参数错误<br>403:访问被拦截<br>500/1500:服务器内部错误<br>503:服务暂不可用 |
| message   | 返回描述                 | string |  Y   | 返回描述                                                     |
| hint      | 返回错误说明             | string |  N   | 返回具体错描述指导                                           |
| seqno     | 服务器日志标示           | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| url       | 优惠码链接               | string |  N   | 查日志用到查问题尽量提供这个值                               |
| expire_in | 优惠码多久之后过期单位秒 | string |  N   | 300                                                          |


### 2.7) 请求返回结果示例:

```
正常返回
{
	"code": "1001",
	"seqno": "573ec9b07a67db60",
	"data_node": "CN-South/HS3-3",
	"time_cost": 36,
	"payload": {
		"url": "https://qr.4pyun.com/discount?token=5853B2C7E4CD4475BE3BFFF1CB213",
		"expire_in": 600
	}
}
```

```
参数错误
{
	"code": "400",
	"message": "请求参数错误",
	"hint": "`store_code` Required!",
	"seqno": "94929a9b0874aa46",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/parking/mcoupon/token"
}
```

```
服务器内部错误
{
	"code": "500",
	"message": "优惠券:7cb7cc3e4e27b8不存在",
	"seqno": "9f46564054d4e4ef",
	"data_node": "CN-South/HS3-3",
	"path": "POST /gate/1.0/parking/mcoupon/token"
}
```
