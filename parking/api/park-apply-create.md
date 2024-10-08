# 提交车场进件

## 1. 接口规则：
### 1.1 应用ID与密钥

AppId是所有P云接口的公共参数，每个开发者会由平台分配唯一的AppId, 与AppId唯一对应的AppSecret则用于生成接口参数的签名，所有接口请求需传递AppId以及签名Sign。



### 1.2 签名规则

P云接口签名规则根据HTTP请求**提交数据方式**的不同而有所差异。

对于通过HTTP Body以格式为**application/json**传参的请求，签名规则如下：

1. 将密钥以“&”为分隔符添加到请求体中的JSON字符串后面，生成待签名字符串，如：{"a"="string", "b"=0, "c"= 1900000109}&app_secret=7d15453e97507ef794cf7b0519d
2. 使用标准MD5算法对待签名字符串进行签名
3. 将签名值写入请求头中的“Authorization”字段

POST application/json请求示例代码：

```java
				String appId = "op010728c14869c8bf4";
        String appSecret = "79B0F3EJF83JF272D9E74FABD95EDE";

        Map<String, Object> params = new HashMap<>();
        params.put("app_id", appId);
        params.put("park_uuid", "e24deadf-1aa0-4981-bde5-f9c474c4f5f5");
        String jsonStr = JSON.toJSONString(params);

        String encryptStr = jsonStr + "&app_secret=" + appSecret;
        String sign = MD5.encryptHEX(encryptStr);

        Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/park/apply")
                .setHeader("Authorization", sign)
                .bodyString(jsonStr, ContentType.APPLICATION_JSON)
                .execute();
```

<h1 id=2.1></h1>


## 2. 停车场进件接口：

### 2.1 停车场创建接口

*https://api.4pyun.com/gate/1.0/parking/park/apply*

- 说明：创建停车场

- 请求方法：POST

- 请求参数：

| 参数名            | 必填 | 说明                                 | 类型   | 示例          |
| :---------------- | :--- | :----------------------------------- | :----- | :------------ |
| app_id            | 是   | 平台分配应用ID                       | string | op72****24    |
| parent            | 是   | 平台分配厂商商户号                   | string | PV3***4       |
| park_name         | 是   | 车场名称，在平台中要求唯一，不可重复 | string | 测试-停车场   |
| address           | 是  | 详细地址                             | string | 讯美科技广场  |
| coordinate        | 是   | 经纬度坐标                           | object |               |
| parking\_space    | 是   | 车位数                               | int  | 50            |
| manager_mobile | 是   | 密保手机号，将用于创建超管账号       | string | 13600000162   |
| service_phone     | 是   | 客服电话                             | string | 0755-28823700 |
| type              | 是   | 车场分类：RESIDENCE-住宅；OFFICE-写字楼；MALL-商业；PUBLIC_PLACE-公共；OTHER-其他；TEST-测试； | string | TEST |
| software_code     | 是 | 软件编码（联系技术确认）                 | string | PO9314 |
| software_param     | 是  | 软件参数, 具体参数由软件编码决定（补充说明联系技术确认） | map<string, string> | {"app_secret": "lKdPpkhXUfeCyPXI"} |



| coordinate | 必填 | 说明                                                         | 类型   | 示例       |
| :--------- | :--- | :----------------------------------------------------------- | :----- | :--------- |
| type       | 是   | 坐标类型：WGS84-地球坐标系；BD09-百度坐标系；GCJ02-火星坐标系； | string    | GCJ02      |
| latitude   | 是   | 纬度                                                         | double | 22.690244  |
| longitude  | 是   | 经度                                                         | double | 113.937153 |




- 请求示例：

 ```json
{
  "parent" : "PV376764",
  "service_phone" : "13800138000",
  "address" : "广东省深圳市宝安区坑尾大道1号石岩公馆",
  "coordinate" : {
    "latitude" : 22.690244,
    "type" : "GCJ02",
    "longitude" : 113.937153
  },
  "software_code" : "PO9314",
  "parking_space" : 1,
  "manager_mobile" : "159****0963",
  "type" : "OTHER",
  "park_name" : "测试进件230404_1",
  "app_id" : "op00961963581daa7",
  "software_param" : {
    "app_secret" : "lKdPpkhXUfeCyPXI"
  }
}
 ```


- 返回参数：

| payload参数名   | 说明       | 类型   | 示例                                 |
| :-------------- | :--------- | :----- | :----------------------------------- |
| park_code       | 车场商户号 | string | 18695715                             |
| park_uuid       | 车场UUID   | string | 6e431a23-08cc-4b36-ad03-79f6fa643717 |
| park_name       | 车场名称   | string | 测试进件230404_1-停车场              |
| manager_account | 超管账号   | string | 18695715                             |
| manager_mobile  | 密保手机号 | string | 159****0963                          |


- 返回示例

 ```json
{
    "code": "1001",
    "seqno": "89515976959588414145524545375641",
    "data_node": "CN-South/DEV-1",
    "time_cost": 880,
    "payload": {
        "park_code": "18695715",
        "park_uuid": "6e431a23-08cc-4b36-ad03-79f6fa643717",
        "park_name": "测试进件230404_1-停车场",
        "manager_account": "18695715",
        "manager_mobile": "15914040963"
    }
}
 ```



## 3. 支付渠道申请链接：

### 3.1 链接传参说明

**跳转链接示例**：https://mch.4pyun.com/external/payment/channel/apply?merchant=xxx&token=xxx

**说明**：跳转链接前需保证车场已在平台创建(调用[2.1 停车场创建接口](#2.1))

**URL**: https://mch.4pyun.com/external/payment/channel/apply

**参数**：

- merchant: P云平台车场商户号(调用[2.1 停车场创建接口](#2.1)返回park_code参数)
- token: 请求令牌



### 3.2 令牌生成规则

```javascript
let content = JSON.stringify({
                iss: 'op01958c1095c8bf4', // 应用ID
                iat: (+new Date() / 1000).toFixed(0), // 令牌生成时间戳(单位秒)
                exp: ((+new Date() + 24 * 60 * 60 * 1000) / 1000).toFixed(0), // 令牌过期时间(单位秒)
            })
let secret = "79B0F3E6E956A6F272D9E74FABD95EDE"; //应用ID密钥
let signature = MD5.hashBinary(content + "&key=" + secret); // 生成签名
let token = "Basic " + Base64.encode(content) + "." + signature; //生成令牌(注意"Basic"后有一空格)
token = encodeURI(token) // UrlEncode
```
