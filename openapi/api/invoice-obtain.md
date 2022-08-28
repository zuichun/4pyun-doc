# 提交开票申请


###  请求地址

    https://api.4pyun.com/gate/1.0/invoice/obtain

###  调用方式

	HTTP POST FORM 表单提交

###  特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:buyer_telephone buyer_address 要么都有值要么都没有值
	3:buyer_bank_name,buyer_bank_account 要么都有值要么都没有值
	4:企业开票也就是buyer_tax_type=1 buyer_tax_no 必须传 buyer_name必须是公司的名称不然开出来的票可能报销不了
	5:企业开票也就是buyer_tax_type=2 buyer_tax_no 不传 buyer_name客户随便填不能有空格但是必须传该字段


### 请求参数

| 字段名称           | 字段说明                                                     |  类型  | 必填 | 示例                             |
| :----------------- | :----------------------------------------------------------- | :----: | :--: | :------------------------------- |
| app_id             | 平台分配的接入应用ID                                         | string |  Y   | op1234567723122                  |
| sign               | 请求数据签名                                                 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant           | 停车场商户号                                                 | string |  Y   | 6262666666                       |
| obtain_order       | 合作方请求开票订单号, 要求同一app_id下唯一                   | string |  Y   | obtainOrder-XXXXXXXXXXXXX        |
| value              | 开票金额(单位分)                                             | string |  Y   | 1000                             |
| buyer_tax_type     | 1:企业开票 2:个人开票                                        | string |  Y   | 1                                |
| buyer_tax_no       | 购方(车主)公司纳税识别号                                     | string |  N   | XXXXXXXXXXXXXXX                  |
| buyer_name         | 购方名称 如果传了购方税号一定要填购方公司名称如果没有传购方税号可以随意传但是不能为空 | string |  Y   | 深圳市神州XXXXXXXXX有限公司      |
| buyer_email        | 接收开票结果邮箱                                             | string |  Y   | 1916714111111@qq.com             |
| buyer_mobile       | 接收开票成功短信手机号,并不是所有开票商都支持发短信建议有就传 | string |  N   | 18075521111                      |
| buyer_telephone    | 购买方公司联系电话                                           | string |  N   | 075588888888                     |
| buyer_address      | 购买方公司联系地址                                           | string |  N   | XXXXXXX                          |
| buyer_bank_name    | 购买方公司银行卡名称                                         | string |  N   | 银行名称                         |
| buyer_bank_account | 购买方公司银行卡账号                                         | string |  N   | 银行账户                         |
| notify_url         | 开票成功或者失败的回调后台地址                               | string |  Y   | https://www.hello.com            |
| extra              | 附加业务参数, 用于生成发票备注模版                           | string |  N   | {"memo":"发票备注"}              |
|                    |                                                              |        |      |                                  |

###  请求示例
```
1.1：签名前字符串
    str=app_id=app_id=op6619067c70f1234213&buyer_email=191671412@qq.com&buyer_mobile=18075521111&buyer_name=随便开票&buyer_tax_no=91440300067123423&buyer_tax_type=1&merchant=626266016&obtain_order=123456789111&value=100&app_secret=d6d3ea8f910b9a3ffa2341234
1.2：MD5(str)
    sign=B10DE5E8A1B5E8B16878AE49C917C212
   @Test
    public void testObtain() {
        TreeMap<String, String> map = new TreeMap<>();
        // 平台分配的接入应用ID
        map.put("app_id", "op6619067c70f1234213");
        // 停车场商户号
        map.put("merchant", "626266016");
        // 合作方请求开票订单号, 要求同一app_id下唯一
        map.put("obtain_order", "123456789111");
        // 开票金额(单位分)
        map.put("value", "100");
        // 1:企业开票 2:个人开票
        map.put("buyer_tax_type", "1");
        // 购方(车主)公司纳税识别号
        map.put("buyer_tax_no", "91440300067123423");
        // 购方名称 如果传了购方税号一定要填购方公司名称如果没有传购方税号可以随意传但是不能为空
        map.put("buyer_name", "随便开票");
        // 接收开票结果邮箱
        map.put("buyer_email", "191671412@qq.com");
        // 接收开票成功短信手机号,并不是所有开票商都支持发短信建议有就传
        map.put("buyer_mobile", "18075521111");

        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "d6d3ea8f910b9a3ffa2341234";
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
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/invoice/obtain")
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


### 请求返回结果参数说明
| 字段名称      | 字段说明       |  类型  | 必填 | 备注                                                       |
| :------------ | :------------- | :----: | :--: | :--------------------------------------------------------- |
| code          | 请求状态码     | string |  Y   | 1001:开票成功<br>1000:开票受理成功<br>其它:读取message信息 |
| message       | 返回描述       | string |  Y   | 返回描述                                                   |
| hint          | 返回错误说明   | string |  N   | 返回具体错描述指导                                         |
| seqno         | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                             |
| obtain_serial | 平台开票编号   | string |  N   | XXXXXXXXX                                                  |


### 请求返回结果示例:

```
正常返回
{
	"code": "1000",
	"message": "开票中",
	"seqno": "3e6af71d2a4e7eec",
	"data_node": "CN-South/HS3-3",
	"time_cost": 838,
	"payload": {
		"obtain_serial": "202107212029510755019876"
	}
}
```

```
正常返回
{
	"code": "1001",
	"message": "开票成功",
	"seqno": "3e6af71d2a4e7eec",
	"data_node": "CN-South/HS3-3",
	"time_cost": 838,
	"payload": {
		"obtain_serial": "202107212029510755019876"
	}
}
```

```
服务器内部错误
{
	"code": "500",
	"message": "商户号不支持电子发票",
	"seqno": "b92e71a7ee445885",
	"data_node": "CN-South/HS3-3",
	"path": "POST /gate/1.0/invoice/obtain"
}
```
