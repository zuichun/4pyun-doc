# 小程序插件支付接入

## 接口说明

|接口|调用方|必须接入|说明|
|---|---|---|---|
|[交易预请求](./../api/trade-prepare.html)|商户|Y|下单获取支付ID|
|[支付结果异步通知](./../api/trade-notify.html)|平台|Y|支付成功同步交易结果到商户|


## 1.添加插件

使用插件前，在项目根目录app.json中声明本插件(根据微信开发者工具控制台提示添加插件）
注意：插件版本号更新后开发工具会有提示，可根据提示修改为最新的插件版本号

代码示例：

```
{
  "plugins": {
    "pay-plugin": {
      "version": "1.0.7",  //引入插件最新版本号
      "provider": "wx5beb76c5c126875c"  //插件的appid
    }
  }
}

```

## 2.使用插件自定义组件

**需携带参数 ：pay_id=xxxxxxxxxxxxxx" 支付订单编号**

在需要跳转到插件页面的 wxml 中**，url 使用 plugin:// 前缀**，形如 plugin://PLUGIN_NAME/PLUGIN_PAGE， 如：

代码示例：
```html
<navigator url="plugin://pay-plugin/Create?pay_id=xxxxxxxxxxxxxx">
    <button type="primary">去缴费</button>
</navigator>
```
