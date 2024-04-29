# 小程序支付接入


## 1 接口说明

|接口|调用方|必须接入|说明|
|---|---|---|---|
|[交易预请求](./../api/trade-prepare.html)|商户|Y|下单获取支付ID|
|[支付结果异步通知](./../api/trade-notify.html)|平台|Y|支付成功同步交易结果到商户|


## 2 接入方式

P云支持通过以下方式接入，选择你需要的方式接入：
- 跳转半屏小程序
- 小程序插件

### 2.1 跳转半屏小程序


#### 2.1.1 配置半屏跳转小程序信息

如果需要实现跳转半屏小程序，需先在app.json做以下配置，[详见文档](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/openEmbeddedMiniProgram.html)

```json
{
  "embeddedAppIdList": ["wxe5f52902cf4de896"]
}
```

#### 2.1.2 通过半屏跳转支付

然后在需要跳转的时候添加以下代码，[详见文档](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.openEmbeddedMiniProgram.html)

```js
wx.openEmbeddedMiniProgram({
  appId: 'wxa204074068ad40ef',
  path: `/external/payment/trade/create/index?pay_id=xxxxxxxxxxxxxx`
})
```

#### 2.1.3 支付完成回调

支付成功后会返回对应的参数

| 字段       | 类型   | 必须 | 说明                                 |
| ---------- | ------ | ---- | ------------------------------------ |
| pay_order  | string | Y    | 支付订单号                           |
| channel    | string | Y    | 支付渠道                             |
| pay_serial | string | Y    | 平台支付流水                         |
| value      | string | Y    | 支付金额，单位：分                   |
| status     | short  | Y    | 交易状态：1支付成功、-1失败、0支付中 |
| trade_time | long   | Y    | 交易时间                             |


### 2.2 小程序插件支付接入

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
```
