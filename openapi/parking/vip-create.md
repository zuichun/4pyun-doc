

## 1) 添加停车场固定车

### 1.1) 请求地址

	https://api.4pyun.com/gate/1.0/parking/vip

### 1.2) 调用方式

	HTTP POST FORM 表单提交

### 1.3) 特殊说明
	1:app_id,app_secret 用户身份id和加密密钥由平台方提供，对接方需提供公司全称然后给到商务提交给研发申请
	2:api_charge_type是停车场软件白名单对应规则ID由平台分配
	3:type固定传0白名单类型


### 1.4) 请求参数

| 字段名称        | 字段说明                                                  |  类型  | 必填 | 示例                             |
| :-------------- | :-------------------------------------------------------- | :----: | :--: | :------------------------------- |
| app_id          | 平台分配的接入应用ID                                      | string |  Y   | op1234567723122                  |
| sign            | 请求数据签名                                              | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| merchant        | 停车场商户号                                              | string |  Y   | 6262666666                       |
| plate           | 车牌号码                                                  | string |  Y   | 粤TXXXXX                         |
| realname        | 车主姓名                                                  | string |  Y   | 张三                             |
| mobile          | 车主联系电话                                              | string |  Y   | 18075528XXX                      |
| id_card         | 车主身份证号码                                            | string |  N   | 431028XXXXXXXXXXXX               |
| start_time      | 生效时间                                                  | string |  Y   | 2020-12-31T16:00:15Z             |
| end_time        | 到期时间                                                  | string |  Y   | 2020-12-31T16:00:15Z             |
| api_charge_type | 接口固定车类型ID                                          | string |  Y   | 12345                            |
| address         | 车主单位/住宅                                             | string |  Y   | XXX栋XXX楼XXX公司                |
| park_space_no   | 车位号                                                    | string |  N   | 负一楼003                        |
| card_id         | 卡ID                                                      | string |  N   | 12345                            |
| type            | 1:月卡2:储值卡 0:免费/白名单                              | string |  Y   | 0                                |
| plate_color     | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知 | string |  Y   | 1                                |

### 1.5）请求示例
```
签名前字符串
1.1：签名前字符串
    str=address=XXX栋XXX楼XXX公司&api_charge_type=12345&app_id=op1234567723122&end_time=2020-12-31T16:00:15Z&merchant=62626601&mobile=1807552XXXX&park_space_no=B0001&plate=粤TXXXXX&plate_color=1&realname=张三&start_time=2020-12-31T16:00:15Z&type=0&app_secret=123

1.2：MD5(str)
    sign=975FA7F729CD413CDB24CC6AE24A63EF
     @Test
    public void testVipCreate(){
        TreeMap<String, String> map = new TreeMap<>();
        // app_id平台分配
        map.put("app_id", "op1234567723122");
        // 停车场商户号
        map.put("merchant", "62626601");
        // 停车场商户号
        map.put("plate", "粤TXXXXX");
        // 车主姓名
        map.put("realname", "张三");
        // 车主联系电话
        map.put("mobile", "1807552XXXX");
        // 生效时间
        map.put("start_time", "2020-12-31T16:00:15Z");
        // 到期时间
        map.put("end_time", "2020-12-31T16:00:15Z");
        // 接口固定车类型ID
        map.put("api_charge_type", "12345");
        // 车主单位/住宅
        map.put("address", "XXX栋XXX楼XXX公司");
        // 车位号
        map.put("park_space_no", "B0001");
        // 1:月卡2:储值卡 0:免费/白名单
        map.put("type", "0");
        // 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知
        map.put("plate_color", "1");
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
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/vip")
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
| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                |
| :------- | :------------- | :----: | :--: | :-------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:添加成功<br>400:参数错误<br>500:服务器内部错误 |
| message  | 返回描述       | string |  Y   | 返回描述                                            |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                  |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                      |


### 1.7) 请求返回结果示例:

```
正常返回
{
  "code": "1001",
  "data_node": "HS1-1",
  "seqno": "86213123",
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
	"path": "POST /gate/1.0/parking/vip"
}
```

```
服务器内部错误
{
	"code": "500",
	"message": "车牌已经是固定车请删除重新添加",
	"seqno": "57804ae2e859f58a",
	"data_node": "CN-South/HS3-1"
}
```
