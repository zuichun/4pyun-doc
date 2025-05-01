# 常见问题

#### Q. SDK链接出现`getaddreinfo` Failed
A. 该原因是本地SDK解析服务器域名失败, 确认：

- 检查域名是否填写正确
- 本地PING域名是否正常
- 检查本地DNS设置是否正确，如果还是无法解决尝试修改本地DNS为阿里云DNS: 223.5.5.5

https://www.alidns.com/
