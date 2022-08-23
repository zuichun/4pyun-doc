#### 1.添加插件

目前本支付插件暂未上架到插件市场，使用插件前，在项目根目录 app.json 中声明本插件(根据微信开发者工具控制台提示添加插件） **需要申请并等待插件开发者通过后**，方可在小程序中使用相应的插件。

```
代码示例：
{
  "plugins": {
    "pay-plugin": {
      "version": "1.0.3",  //引入插件最新版本号
      "provider": "wx5beb76c5c126875c"  //插件的appid
    }
  }
}

项目根目录 app.json
```

#### 2.使用插件自定义组件

**需携带参数 ：pay_id=xxxxxxxxxxxxxx" 支付订单编号**

在需要跳转到插件页面的 wxml 中**，url 使用 plugin:// 前缀**，形如 plugin://PLUGIN_NAME/PLUGIN_PAGE， 如：

```
代码示例：

<navigator url="plugin://pay-plugin/Create?pay_id=xxxxxxxxxxxxxx">   //跳转本插件需要传递的参数 pay_id 支付订单编号
<button type="primary">去缴费</button>
</navigator>

需要跳转到插件的页面 例如：pages/index/index.wxml
```
