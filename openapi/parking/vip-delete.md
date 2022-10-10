

## 2) 删除停车场固定车

### 2.1) 请求地址

	https://api.4pyun.com/gate/1.0/parking/vip

### 2.2) 调用方式

	HTTP DELETE

### 2.3) 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请


### 2.4) 请求参数

| 字段名称 | 字段说明             |  类型  | 必填 | 示例                             |
| :------- | :------------------- | :----: | :--: | :------------------------------- |
| app_id   | 平台分配的接入应用ID | string |  Y   | op1234567723122                  |
| sign     | 请求数据签名         | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant | 停车场商户号         | string |  Y   | 626266666666                     |
| plate    | 车牌号码             | string |  Y   | 粤TXXXXX                         |
| reason   | 删除原因             | string |  Y   | 员工离职                         |

### 2.5）请求示例
```
签名前字符串
1.1：签名前字符串    str=app_id=op1234567723122&merchant=62626601&plate=粤TXXXXX&app_secret=123
1.2：MD5(str)
    sign=1701B126AE28C8BB6CB4A5D96A3828FE
     @Test
    public void testVipDelete(){
        TreeMap<String, String> map = new TreeMap<>();
        // app_id平台分配
        map.put("app_id", "op1234567723122");
        // 停车场商户号
        map.put("merchant", "62626601");
        // 停车场商户号
        map.put("plate", "粤TXXXXX");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "123";
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
            Response response = Request.Delete("https://api.4pyun.com/gate/1.0/parking/vip?"+keyStr)
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
| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                |
| :------- | :------------- | :----: | :--: | :-------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:删除成功<br>400:参数错误<br>500:服务器内部错误 |
| message  | 返回描述       | string |  Y   | 返回描述                                            |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                  |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                      |


### 2.7) 请求返回结果示例:

```
正常返回
{
	"code": "1001",
	"seqno": "573ec9b07a67db60",
	"data_node": "CN-South/HS3-3",
	"time_cost": 36
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
	"path": "DELETE /gate/1.0/parking/vip"
}
```

```
服务器内部错误
{
	"code": "500",
	"message": "车牌号码不是固定车",
	"seqno": "57804ae2e859f58a",
	"data_node": "CN-South/HS3-1"
}
```
