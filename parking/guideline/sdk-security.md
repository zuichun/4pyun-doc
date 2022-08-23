# P云SDK安全更新指引

为保障各位厂商软件利益，防止软件被第三方滥用，请各位厂商升级新版本SDK到v1.0.6.32+
升级步骤
1. 前往 https://gitee.com/chinaroad/4pyun-api/releases 下载最新版本更新本地4pyun-api.dll文件;
2. 为防止通过替换低版本绕过验证，请各位厂商通过调用 PYunAPILevel 方法判断返回值>=10才允许启动SDK, 否则直接拒绝启动SDK。

```c
// 获取SDK API等级, 必须执行这个步骤
int sdk_level = PYunAPILevel();
if (sdk_level < 10) {
    // 过低的SDK版本
    return -1;
}
```

说明
- 更新后SDK在执行启动前会验证配置的域名端口是否合法, 不合法将拒绝建立链接；
- 当SDK链接到服务端后，SDK将验证当前服务端返回到客户端地址是否合法，若不合法也将直接关闭链接。
