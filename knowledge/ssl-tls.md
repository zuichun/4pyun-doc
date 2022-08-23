# 彻底搞清楚SSL/TLS

## 1 TLS/SSL的前世今生

SSL(Secure Sockets Layer)最初由`Netscape`定义, 分别有SSLv2和SSLv3两个版本(SSLv1未曾对外发布); 在SSLv3之后SSL重命名为TLS。

TLS(Transport Layer Security)版本从TLSv1.0开始, TLSv1.0是在SSLv3的基础上升级而来。

|协议|时间|建议|说明|
|---|---|--|---|
|SSLv1|/|/|实际从未公开发布。|
|SSLv2|1995|弃用|IETF已于2011年弃用。|
|SSLv3|1996|弃用|IETF已于2015年弃用。|
|TLSv1.0|1999|兼容|-|
|TLSv1.1|2006|兼容|-|
|TLSv1.2|2008|主推|目前最新可用版本|
|TLSv1.3|/|/|2016开始草案制定|

多年以来已弃用的SSL协议也暴露出了一些高危漏洞(例如: POODLE
, DROWN); 因此建议服务器禁用SSL3.0及SSL2.0, 只启用TLS协议。

## 2 证书如何工作

SSL/TLS使用证书来实现对数据的加密传输以及身份认证。

## 3 TLS握手过程

![diagram](./media/tls-handshake.png)

### 3.1 导致握手失败的一些原因

- 两边协议版本不兼容
- 两边加密算法无匹配项


# 附录

[1] RFC6176 - Prohibiting Secure Sockets Layer (SSL) Version 2.0

https://tools.ietf.org/html/rfc6176

[2] RFC7568 - Deprecating Secure Sockets Layer Version 3.0

https://tools.ietf.org/html/rfc7568

[3] RFC2246 - The TLS Protocol Version 1.0

https://tools.ietf.org/html/rfc2246

[4] RFC4346 - The Transport Layer Security (TLS) Protocol Version 1.1

https://tools.ietf.org/html/rfc4346

[5] RFC5246 - The Transport Layer Security (TLS) Protocol Version 1.2

https://tools.ietf.org/html/rfc5246

[6] RFC2246 - The TLS Protocol Version 1.0

https://tools.ietf.org/html/rfc2246

[7] RFC4346 - The Transport Layer Security (TLS) Protocol Version 1.1

https://tools.ietf.org/html/rfc4346

[8] RFC5246 - The Transport Layer Security (TLS) Protocol Version 1.2

https://tools.ietf.org/html/rfc5246

[9] The Transport Layer Security (TLS) Protocol Version 1.3

https://tools.ietf.org/html/draft-ietf-tls-tls13-13

[10] SSL and TLS Protocols

https://wiki.openssl.org/index.php/SSL_and_TLS_Protocols
