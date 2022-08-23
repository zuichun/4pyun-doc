# 正确处理HTTPS证书

## Java中HTTPS会遇到的问题
- 访问自签名的HTTPS网站
- 高版本JRE访问SSLv3/SSLv2站点
- 一些银行接口需要加载keystore的场景

## 1 访问自签名的HTTPS网站

常常看到的回答是直接通过信任所有来支持, 这不优雅; 优雅的操作应该:

- 下载服务端的CA证书

```sh
# 方式1: 导出DER格式的证书
# 这里需要通过指定servername来保证导出的证书和当前域名匹配
openssl s_client -showcerts -connect self-signed.badssl.com:443 -servername self-signed.badssl.com </dev/null 2>/dev/null|openssl x509 -outform der >self-signed.badssl.com.der

```

- 代码中通过加载服务端证书后通过自定义SSLContext访问目标服务器:

```java
private SSLContext sslContext(File certificateFile, String certificateType) {

    InputStream inputStream = null;
    try {
        inputStream = new FileInputStream(certificateFile);
        CertificateFactory cf = CertificateFactory.getInstance(certificateType);
        Certificate certificate = cf.generateCertificate(inputStream);
        System.out.println("ca=" + ((X509Certificate) certificate).getSubjectDN());
        String alias = ((X509Certificate) certificate).getSubjectDN().toString();

        KeyStore keyStore = KeyStore.getInstance(KeyStore.getDefaultType());
        keyStore.load(null, null);
        keyStore.setCertificateEntry(alias, certificate);

        // Create a KeyStore containing our trusted CAs
        SSLContext sslcontext = SSLContexts.custom()
                .loadTrustMaterial(keyStore, new TrustSelfSignedStrategy())
                .build();
        return sslcontext;
    } catch (IOException e) {
        throw new RuntimeException(e);
    } catch (CertificateException e) {
        throw new RuntimeException(e);
    } catch (NoSuchAlgorithmException e) {
        throw new RuntimeException(e);
    } catch (KeyStoreException e) {
        throw new RuntimeException(e);
    } catch (KeyManagementException e) {
        throw new RuntimeException(e);
    } finally {
        if (inputStream != null) try { inputStream.close(); } catch (IOException e) { }
    }
}

@Test
public void testSelfSign() throws IOException {
    File certFile = ResourceUtils.getFile(this.getClass().getResource("/root.cer"));

    CloseableHttpClient httpclient = HttpClients.custom()
            .setSSLContext(sslContext(certFile, "X.509"))  //
            .build();

    String body = Executor.newInstance(httpclient).execute(Request
            .Get("https://self-signed.badssl.com/"))
            .returnContent()
            .asString();
    System.out.println(body);
}
```

## 2 高版本JRE访问SSLv3/SSLv2站点

通常你得到的答案是通过修改`${JRE_HOME}/lib/security/java.security`目录下某些配置项来取消高版本SDK对某些不安全SSL协议版本或算法的限制。

各版本对SSL的支持情况

||JDK8|JDK7|JDK6|
|---|---|---|---|
|TLS Protocols|TLSv1.2 (default)<br/>TLSv1.1<br/>TLSv1<br/>SSLv3|TLSv1.2<br/>TLSv1.1<br/>TLSv1 (default)<br/>SSLv3|TLS v1.1 (JDK 6 update 111 and above)<br/>TLSv1 (default)<br/>SSLv3|
|JSSE Ciphers:|[Ciphers in JDK 8](https://docs.oracle.com/javase/8/docs/technotes/guides/security/StandardNames.html#ciphersuites)|[Ciphers in JDK 7](https://docs.oracle.com/javase/7/docs/technotes/guides/security/StandardNames.html#ciphersuites)|[Ciphers in JDK 6](https://docs.oracle.com/javase/6/docs/technotes/guides/security/StandardNames.html#SupportedCipherSuites)|
|Reference:|[JDK 8 JSSE](http://docs.oracle.com/javase/8/docs/technotes/guides/security/jsse/JSSERefGuide.html)|[JDK 7 JSSE](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/JSSERefGuide.html)|[JDK 6 JSSE](http://docs.oracle.com/javase/6/docs/technotes/guides/security/jsse/JSSERefGuide.html)|
|Java Cryptography Extension, Unlimited Strength (explained later)|[JCE for JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)|[JCE for JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html)|[JCE for JDK 6](http://www.oracle.com/technetwork/java/javase/downloads/jce-6-download-429243.html)|

\* 在2015年1月发布的升级补丁中也已经禁用对SSLv3的支持。

所以为什么会出现在某些高版本无法访问某些HTTPS站点的原因就是由于有以下可能:
- 服务端支持对SSL版本在本地JRE已经被禁用, 例如服务端只支持SSLv3而JDK已经默认关闭了对SSLv3的支持。
- 服务端使用的`JSSE Ciphers`和本地支持的`JSSE Ciphers`没有共同项导致无法正常选择加密算法。

### 2.1 确认当前JRE启用SSL协议

```java
@Test
public void sslSupport() throws IOException {
    SSLSocketFactory factory = (SSLSocketFactory) SSLSocketFactory.getDefault();
    SSLSocket soc = (SSLSocket) factory.createSocket();

    // Returns the names of the protocol versions which are
    // currently enabled for use on this connection.
    String[] protocols = soc.getEnabledProtocols();

    System.out.println("Enabled protocols:");
    for (String s : protocols) {
        System.out.println(s);
    }
}
```
输出:
```txt
Enabled protocols:
TLSv1
TLSv1.1
TLSv1.2
```

### 2.2 确认当前JRE启用CipherSutes
```java
String[] cipers = soc.getEnabledCipherSuites();
System.out.println("Enabled CipherSutes:");
for (String s : cipers) {
    System.out.println(s);
}
```
输出
```txt
Enabled CipherSutes:
TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
TLS_RSA_WITH_AES_256_CBC_SHA256
TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA384
TLS_ECDH_RSA_WITH_AES_256_CBC_SHA384
TLS_DHE_RSA_WITH_AES_256_CBC_SHA256
TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
TLS_RSA_WITH_AES_256_CBC_SHA
TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA
TLS_ECDH_RSA_WITH_AES_256_CBC_SHA
TLS_DHE_RSA_WITH_AES_256_CBC_SHA
TLS_DHE_DSS_WITH_AES_256_CBC_SHA
TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
TLS_RSA_WITH_AES_128_CBC_SHA256
TLS_ECDH_ECDSA_WITH_AES_128_CBC_SHA256
TLS_ECDH_RSA_WITH_AES_128_CBC_SHA256
TLS_DHE_RSA_WITH_AES_128_CBC_SHA256
TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
TLS_RSA_WITH_AES_128_CBC_SHA
TLS_ECDH_ECDSA_WITH_AES_128_CBC_SHA
TLS_ECDH_RSA_WITH_AES_128_CBC_SHA
TLS_DHE_RSA_WITH_AES_128_CBC_SHA
TLS_DHE_DSS_WITH_AES_128_CBC_SHA
TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
TLS_RSA_WITH_AES_256_GCM_SHA384
TLS_ECDH_ECDSA_WITH_AES_256_GCM_SHA384
TLS_ECDH_RSA_WITH_AES_256_GCM_SHA384
TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
TLS_DHE_DSS_WITH_AES_256_GCM_SHA384
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
TLS_RSA_WITH_AES_128_GCM_SHA256
TLS_ECDH_ECDSA_WITH_AES_128_GCM_SHA256
TLS_ECDH_RSA_WITH_AES_128_GCM_SHA256
TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
TLS_DHE_DSS_WITH_AES_128_GCM_SHA256
TLS_EMPTY_RENEGOTIATION_INFO_SCSV
```

### 2.3 检查服务器支持的SSL协议

这里我推荐使用`nmap`检测:
```sh
nmap --script ssl-enum-ciphers -p 443 badssl.com
```
输出:
```txt
Starting Nmap 7.70 ( https://nmap.org ) at 2019-01-01 22:26 CST
Nmap scan report for badssl.com (104.154.89.105)
Host is up (0.32s latency).
rDNS record for 104.154.89.105: 105.89.154.104.bc.googleusercontent.com

PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers:
|   TLSv1.0:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 2048) - A
|       TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA (secp256r1) - C
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
|       TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 2048) - A
|       TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 2048) - A
|     compressors:
|       NULL
|     cipher preference: server
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|   TLSv1.1:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 2048) - A
|       TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA (secp256r1) - C
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
|       TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 2048) - A
|       TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 2048) - A
|     compressors:
|       NULL
|     cipher preference: server
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 2048) - A
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp256r1) - A
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA256 (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA256 (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 2048) - A
|       TLS_ECDHE_RSA_WITH_3DES_EDE_CBC_SHA (secp256r1) - C
|       TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_GCM_SHA384 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA256 (rsa 2048) - A
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
|       TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (rsa 2048) - A
|       TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA (dh 2048) - A
|       TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (rsa 2048) - A
|     compressors:
|       NULL
|     cipher preference: server
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|_  least strength: C

Nmap done: 1 IP address (1 host up) scanned in 39.91 seconds
```
根据输出可以看到`badssl.com`同时支持`TLSv1.0`、`TLSv1.1`以及`TLSv1.2`, 同时也可以看到当前对应协议支持的加密算法。

### 2.4 解决方案

JRE在`${JRE_HOME}/lib/security/java.security`配置了一些算法的配置, 例如本地我的`${JRE_HOME}/lib/security/java.security`配置内容为:

```txt
jdk.tls.disabledAlgorithms=SSLv3, RC4, DES, MD5withRSA, DH keySize < 1024, \
    EC keySize < 224, 3DES_EDE_CBC
```
表示当前JRE要禁用`SSLv3`协议以及`RC4`、`DES`等算法。

当然我们可以通过手动修改该文件来取消这些限制来达到我们对目的, 但这样程序在部署到新环境就可能不能正常运行, 不优雅! 优雅对操作如下:

- 根据协议版本动态对JRE对设置取消设置, 在JRE中管理相关SSL协议以及算法的配置项主要是`jdk.tls.disabledAlgorithms`。

```java
// 取消当前运行环境对SSLv3、RC4、DES以及3DES_EDE_CBC的禁用限制
static {
    String disabledAlgorithms = Security.getProperty("jdk.tls.disabledAlgorithms");
    HashSet<String> keys = Sets.newHashSet(disabledAlgorithms.split(", +"));
    if (keys.contains("SSLv3")) keys.remove("SSLv3");
    if (keys.contains("RC4")) keys.remove("RC4");
    if (keys.contains("DES")) keys.remove("DES");
    if (keys.contains("3DES_EDE_CBC")) keys.remove("3DES_EDE_CBC");

    Security.setProperty("jdk.tls.disabledAlgorithms", StringUtils.join(keys, ", "));
    log.debug("SECURITY PROPERTY UPDATED \"jdk.tls.disabledAlgorithms\" = " + Security.getProperty("jdk.tls.disabledAlgorithms"));
}
```
- 自定义`SSLConnectionSocketFactory`兼容对低版本协议对支持, 突破JRE默认限制。

```java
// Allow SSLv3, TLSv1, TLSv1.2 protocol only
SSLConnectionSocketFactory sslConnectionSocketFactory =  new SSLConnectionSocketFactory(
        sslContext,
        new String[] { "SSLv3", "TLSv1", "TLSv1.2"},
        null,
        NoopHostnameVerifier.INSTANCE);
```

至此你就可以完美且优雅的解决这个问题。

## 附录

[1] HTTPS<br/>
https://en.wikipedia.org/wiki/HTTPS

[2] Obtain a Certificate from Server<br/>
https://ldapwiki.com/wiki/Obtain%20a%20Certificate%20from%20Server

[3] Transport Level Security (TLS) and Java<br/>
http://www.ateam-oracle.com/tls-and-java/

[4] Diagnosing TLS, SSL, and HTTPS<br/>
https://blogs.oracle.com/java-platform-group/diagnosing-tls,-ssl,-and-https
