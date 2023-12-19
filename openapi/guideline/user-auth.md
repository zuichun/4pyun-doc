# 用户授权

## 构造网页授权链接

如果第三方应用需要在打开的网页里面携带用户的身份信息，第一步需要构造如下的链接来获取code：

```js
https://auth.4pyun.com/authorize?redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE
```

**参数说明**

|参数|必须|说明|
|---|---|---|
|response_type|N|支持:<br>`code`-默认<br>`openid`-用户微信OPENID<br>`identity`用户ID|
|scope|N|应用授权作用域: `snsapi_base`-静默授权|
|redirect_uri|N|授权后重定向的回调链接地址，请使用urlencode对链接进行处理 ，注意域名需要设置为第三方应用的可信域名|

**CASE 1** - 获取用户OPENID

请求连接:

```
https://auth.4pyun.com/authorize?scope=snsapi_base&response_type=openid&redirect_uri=REDIRECT_URI
```
授权后跳转:
```
REDIRECT_URI?openid=XXXXX
```

**CASE 2** - 获取用户IDENTITY

请求连接:

```
https://auth.4pyun.com/authorize?scope=snsapi_base&response_type=identity&redirect_uri=REDIRECT_URI
```
授权后跳转:
```
REDIRECT_URI?identity=XXXXX
```
