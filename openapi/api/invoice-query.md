# 查询开票详情

### 请求地址

	https://api.4pyun.com/gate/1.0/invoice/detail

### 调用方式

	HTTP GET

### 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:状态码code 1001只代表请求结果正常不代表开票成功，开票结果认status


### 请求参数

| 字段名称     | 字段说明                                   |  类型  | 必填 | 示例                             |
| :----------- | :----------------------------------------- | :----: | :--: | :------------------------------- |
| app_id       | 平台分配的接入应用ID                       | string |  Y   | op1234567723122                  |
| sign         | 请求数据签名                               | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant     | 停车场商户号                               | string |  Y   | 6262666666                       |
| obtain_order | 合作方请求开票订单号, 要求同一app_id下唯一 | string |  Y   | obtainOrder-XXXXXXXXXXXXX        |

### 请求示例
```
签名前字符串
1.1：签名前字符串    str=app_id=op6619067c70f1234213&merchant=626266016&obtain_order=123456789111&app_secret=d6d3ea8f910b9a3ff1234234
1.2：MD5(str)
    sign=CF56BA852C2F8DF9CD8D3DFB94C56DC0
     @Test
    public void testInvoiceDetail() {
        TreeMap<String, String> map = new TreeMap<>();
        // 平台分配的接入应用ID
        map.put("app_id", "op6619067c70f1234213");
        // 停车场商户号
        map.put("merchant", "626266016");
        // 合作方请求开票订单号, 要求同一app_id下唯一
        map.put("obtain_order", "123456789111");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "d6d3ea8f910b9a3ff1234234";
        String encriptStr = builder.toString() + "app_secret=" + appSecret;
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        String keyStr = builder.toString() + "sign=" + sign;
        String url = "https://api.4pyun.com/gate/1.0/invoice/detail?";
        System.out.println(url + keyStr);
        try {
            Response response = Request.Get(url + keyStr).execute();
            HttpResponse response1 = response.returnResponse();
            System.out.println(response1.getStatusLine());
            String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
            System.out.println(text);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```


### 请求返回结果参数说明
| 字段名称     | 字段说明                                       |  类型  | 必填 | 备注                                                     |
| :----------- | :--------------------------------------------- | :----: | :--: | :------------------------------------------------------- |
| code         | 请求状态码                                     | string |  Y   | 1001:查询成功<br>1002:订单不存在<br>其它:读取message信息 |
| message      | 返回描述                                       | string |  Y   | 返回描述                                                 |
| hint         | 返回错误说明                                   | string |  N   | 返回具体错描述指导                                       |
| seqno        | 服务器日志标示                                 | string |  Y   | 查日志用到查问题尽量提供这个值                           |
| merchant     | 停车场商户号                                   | string |  N   | 6262666666                                               |
| status       | -1:开票失败 0:等待接口开票 1:开票中 2:开票成功 | string |  N   | 2                                                        |
| status_desc  | 状态说明/开票失败说明                          | string |  N   | 开票成功                                                 |
| invoice_code | 发票代码                                       | string |  N   | 12341234                                                 |
| invoice_no   | 发票号码                                       | string |  N   | 12342314                                                 |
| verify_code  | 发票校验码                                     | string |  N   | 12341234                                                 |
| invoice_url  | 发票链接                                       | string |  N   | https://www.asdfafadf.pdf                                |
| invoice_time | 开票请求时间                                   | string |  N   | 2020-06-10T13:44:25Z                                     |


### 请求返回结果示例:

```
正常返回
{
	"code": "1001",
	"message": "查询成功",
	"seqno": "79bf5a170965c38b",
	"data_node": "CN-South/HS3-3",
	"time_cost": 45,
	"payload": {
		"merchant": "62626601",
		"status": -1,
		"status_desc": "第三方开票失败",
		"invoice_code": "",
		"invoice_no": "",
		"verify_code": "",
		"invoice_url": "",
		"invoice_time": "2020-06-10T13:44:25Z"
	}
}
```

```
参数错误
{
	"code": "1002",
	"message": "未查询到开票记录",
	"seqno": "99fac25f94584fc4",
	"data_node": "CN-South/HS3-2",
	"time_cost": 407,
	"payload": {
		"merchant": "",
		"status": 0,
		"status_desc": "",
		"invoice_code": "",
		"invoice_no": "",
		"verify_code": "",
		"invoice_url": ""
	}
}
```

```
服务器内部错误
{
	"code": "500",
	"message": "我知道错了大神不要再发请求了",
	"seqno": "9f46564054d4e4ef",
	"data_node": "CN-South/HS3-3",
	"path": "GET /gate/1.0/invoice/detail"
}
```
