# 小程序支付接入


<font color="red">
尊敬的开发者，微信小程序P云支付插件已停止维护，正在使用插件的小程序不受影响！请尽快升级到半屏微信小程序起支付，用户体验更佳接入更简单。
</font>


## 1 接口说明

|接口|调用方|必须接入|说明|
|---|---|---|---|
|[交易预请求](./../api/trade-prepare.html)|商户|Y|下单获取支付ID|
|[支付结果异步通知](./../api/trade-notify.html)|平台|Y|支付成功同步交易结果到商户|


## 2 接入方式

### 2.1 跳转半屏微信小程序

#### 2.1.1 配置半屏跳转小程序信息

需先在app.json做以下配置，[详见文档](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/openEmbeddedMiniProgram.html)

```json
{
  "embeddedAppIdList": ["wxa204074068ad40ef"]
}
```

然后在需要跳转的时候添加以下代码，[详见文档](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.openEmbeddedMiniProgram.html)

```js
wx.openEmbeddedMiniProgram({
  appId: 'wxa204074068ad40ef',
  path: `/external/payment/trade/create/index?pay_id=xxxxxxxxxxxxxx`
})
```

#### 2.1.2 支付完成回调

支付成功后会返回对应的参数(目标小程序可在 App.onLaunch，App.onShow 中获取到这份数据。) [详见文档](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.navigateToMiniProgram.html)

| 字段       | 类型   | 必须 | 说明                                 |
| ---------- | ------ | ---- | ------------------------------------ |
| pay_order  | string | Y    | 支付订单号                           |
| channel    | string | Y    | 支付渠道                             |
| pay_serial | string | Y    | 平台支付流水                         |
| value      | string | Y    | 支付金额，单位：分                   |
| status     | short  | Y    | 交易状态：1支付成功、-1失败、0支付中 |
| trade_time | long   | Y    | 交易时间                             |


<!-- ### 2.2 微信小程序插件支付接入

#### 2.2.1 添加插件

使用插件前，在项目根目录app.json中声明本插件(根据微信开发者工具控制台提示添加插件）
注意：插件版本号更新后开发工具会有提示，可根据提示修改为最新的插件版本号

代码示例：

```json
{
  "plugins": {
    "pay-plugin": {
      "version": "1.0.8",  //引入插件最新版本号
      "provider": "wx5beb76c5c126875c"  //插件的appid
    }
  }
}

```

#### 2.2.2 使用插件自定义组件

**需携带参数 ：pay_id=xxxxxxxxxxxxxx" 支付订单编号**

在需要跳转到插件页面的 wxml 中**，url 使用 plugin:// 前缀**，形如 plugin://PLUGIN_NAME/PLUGIN_PAGE， 如：

代码示例：
```html
<navigator url="plugin://pay-plugin/Create?pay_id=xxxxxxxxxxxxxx">
    <button type="primary">去缴费</button>
</navigator>
``` -->

### 2.2 跳转半屏支付宝小程序

#### 2.2.1 跳转小程序信息

然后在需要跳转的时候添加以下代码，[详见文档](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.openEmbeddedMiniProgram.html)

```js
my.openEmbeddedMiniProgram({
        appId: '2021004130621008',
        path: `/external/payment/trade/create/index?pay_id=xxxxxxxxxxxxxx`,
 })
```

#### 2.2.2 支付完成回调

支付成功后会返回对应的参数

| 字段       | 类型   | 必须 | 说明                                 |
| ---------- | ------ | ---- | ------------------------------------ |
| pay_order  | string | Y    | 支付订单号                           |
| channel    | string | Y    | 支付渠道                             |
| pay_serial | string | Y    | 平台支付流水                         |
| value      | string | Y    | 支付金额，单位：分                   |
| status     | short  | Y    | 交易状态：1支付成功、-1失败、0支付中 |
| trade_time | long   | Y    | 交易时间                             |

