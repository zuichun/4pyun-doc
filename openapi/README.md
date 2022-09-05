# 开放平台文档

该部分内容供有云平台研发能力的公司做接口接入，平台开放基础服务能力包括但不限于:

- 聚合支付
- 电子发票
- 广告联盟
- 停车业务

等功能。

您可联系平台商务获得技术以及商务政策信息，平台将安排专人协助完成功能接入。


#### 1.1 公共参数

| 字段       | 说明                                | 示例                             |
| :--------- | :---------------------------------- | :------------------------------- |
| app_id     | 应用ID, 由P云平台分配               | op12defadfad                     |
| timestamp  | 当前请求时间戳, 单位毫秒            | 1570318158852                    |
| sign       | 签名数据，具体规则见签名算法        | 1A6FE20BDD05B654F8FD33A299D75DF3 |
| sign_type  | 签名算法                            | MD5                              |


#### 1.2 签名算法

**算法执行步骤**

1. 设所有发送或接收到的数据为集合M，将集合M内非空参数值的参数按照参数名 `ASCII码` 从小到大排序（字典序），使用 `URL键值对` 的格式（即 `key1=value1&key2=value2...`）拼接成字符串 `stringA`。
2. `sign = stringA + "&app_secret=" + appSecret`，取 `MD5`（`32位不区分大小写`）。

**注意事项**

1. 参数名 ASCII码 从小到大排序（字典序）；
2. 如果参数值为空（即null或空字符串）不参与签名；
3. 参数名区分大小写；
4. 验证签名时，sign 参数不参与签名，将生成的签名与该 sign 值作校验；
5. 接口可能增加字段，验证签名时必须支持增加的扩展字段；

**示例**

**密钥：**

| 字段       | 示例值                           |
| ---------- | -------------------------------- |
| app_id     | op88641899bd20661                |
| app_secret | 29b72e85f56f9d20b2303d5289fe78c9 |

**输入参数：**

| 字段       | 示例值                               |
| ---------- | ------------------------------------ |
| park_uuid  | 40e06b24-7320-4a61-8d97-7ebccb364a87 |
| plate      | 粤B660PP                             |
| car_type   | 1                                    |
| enter_time | 1563242533431                        |

**计算 sign 的过程如下：**

1. 参数排序后字符串拼接：
```
app_id=op88641899bd20661&car_type=1&enter_time=1563242533431&park_uuid=40e06b24-7320-4a61-8d97-7ebccb364a87&plate=粤B660PP&sign_type=MD5&timestamp=1563242932357&app_secret=29b72e85f56f9d20b2303d5289fe78c9
```

2. 使用 MD5 （32位不区分大小写）加密：
```
1A6FE20BDD05B654F8FD33A299D75DF3
```


### 签名规则

签名规则根据HTTP请求**提交数据方式**的不同而有所差异。
对于通过**url传参**，及**form表单传参**的请求，签名规则如下：

1. 先将所有业务参数按照参数名（不包括 sign）进行升序排序（若有多个相同参数名则继续按照其参数值进行升序排序）;
2. 以 'http url' 参数风格拼接成待签名的字符串，如：a=v&b=0&c=1900000109&d=102
3. 再将密钥拼接到待签名的字符串后面，如：a=v&b=0&c=1900000109&d=102&app_secret=7d15453e97507ef794cf7b0519d
4. 使用标准MD5算法对待签名字符串进行签名;
5. 将签名值写入 'sign' 字段;

URL传参请求示例代码：

```java
String appId = "op010728c14869c8bf4";
String appSecret = "79B0F3EJF83JF272D9E74FABD95EDE";

Map<String, String> params = new TreeMap<>();
params.put("app_id",appId);
params.put("park_uuid","e24deadf-1aa0-4981-bde5-f9c474c4f5f5");

StringBuilder builder = new StringBuilder();
for (String key : params.keySet()) {
    builder.append(key + "=" + map.get(key) + "&");
}

String encryptStr = builder.toString() + "app_secret=" + appSecret;
String sign = MD5.encryptHEX(encryptStr);

String paramStr= builder.toString()+"sign="+sign;
String url ="https://api.4pyun.com/gate/1.0/xxxxx?"+ paramStr;

Response response = Request.Get(url).execute();
```

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

Response response = Request.Post("https://api.4pyun.com/gate/1.0/xxxx")
        .setHeader("Authorization", sign)
        .bodyString(jsonStr, ContentType.APPLICATION_JSON)
        .execute();
```
