# 车联网业务



### 接口地址

- 测试环境：`https://dev-api.4pyun.com`

- 生产环境：`https://api.4pyun.com`

### 签名

#### 计算方式

1. 先过滤掉值为空的参数；
2. 在将所有业务参数按照参数名进行升序排序；
3. 以 `HTTP URL` 风格拼接成待签名的字符串，如：`a=1&b=2&c=123`
4. 再将密钥以 `&app_secret=您的密钥` 形式拼接到上一步待签名的字符串后面，如：`a=1&b=2&c=123&app_secret=您的密钥`
5. 使用标准 `MD5` 算法对上述字符串进行签名（忽略大小写）；
6. 将签名值进行 `HTTP` 请求即可。

#### 传递方式

> Content-Type=application/x-www-form-urlencoded

通过 `body` 传递签名。`sign=请求签名值`
