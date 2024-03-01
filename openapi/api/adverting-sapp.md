# 小程序接入广告

## 1.引入npm包
```
npm install 4pyun-ad-sdk
```


## 2.构建
- uni-app框架：[小程序自定义组件支持](https://uniapp.dcloud.net.cn/tutorial/miniprogram-subject.html#%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E6%94%AF%E6%8C%81/)
1. 在根目录新建wxcomponents文件夹
2. 打开node_modules/4pyun-ad-sdk文件，copy目录中整个src文件到wxcomponents目录下，按需重命名使用
3. 目录结构

```
┌─wxcomponents              微信小程序自定义组件存放目录
│   └──4pyun-ad-sdk      微信小程序自定义组件
│        ├─index.js
│        ├─index.wxml
│        ├─index.json
│        └─index.wxss
├─pages
├─main.js
├─App.vue
├─manifest.json
└─pages.json
```

- 小程序原生：[小程序npm支持](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html)
1. 小程序开发者工具 -> 详情(工具右上角) -> 本地设置 -> 使用npm模块
2. 小程序开发者工具 -> 工具 -> 构建 npm
3. 构建成功后小程序代码包中将产出 "miniprogram_npm" 文件夹

## 3.使用
- uni-app框架：[小程序自定义组件支持](https://uniapp.dcloud.net.cn/tutorial/miniprogram-subject.html#%E5%B0%8F%E7%A8%8B%E5%BA%8F%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E6%94%AF%E6%8C%81/)
1. 找到需要引入广告的页面
2. 页面的json文件中做如下配置

```
// pages.json
{
  "pages": [
    {
      "path": "4pyun-ad-sdk/4pyun-ad-sdk,
      "style": {
          "usingComponents": {
              "4pyun-ad-sdk": "/wxcomponents/4pyun-ad-sdk/index"
          }
      }
    }
  ]
}
```

3. 在页面中引入该组件

```
<template>
  <4pyun-ad-sdk
    space_id="{{space_id}}"
    openid="{{openid}}"
    app_id="{{app_id}}"
    provider_code="{{provider_code}}"
    personas="{{personas}}"
  >
  </4pyun-ad-sdk>
</template>

<script>
  export default {
    data() {
        return {
            space_id: '',
            openid: '',
            app_id: '',
            provider_code: '',
            personas: {
                mobile: '131xxxxxxxx',
                plate: '粤BAAAAA',
            }
        }
    }
  }
</script>
```

- 小程序原生
1. 找到需要引入广告的页面
2. 页面的json文件中做如下配置

```
// index.json
{
    "usingComponents": {
        "4pyun-ad-sdk": "4pyun-ad-sdk"
    }
}
```

3. 在页面中引入该组件

```
// index.wxml

<4pyun-ad-sdk
  space_id="{{space_id}}"
  openid="{{openid}}"
  app_id="{{app_id}}"
  provider_code="{{provider_code}}"
  personas="{{personas}}"
>
</4pyun-ad-sdk>

// index.js
data: {
  space_id: '',
  openid: '',
  app_id: ''
  provider_code: '',
  personas: {
    mobile: '131xxxxxxxx',
    plate: '粤BAAAAA',
  }
}
```

## 4.参数说明

| 变量 | 类型 | 必填 | 可选值 | 说明 |
| ---- | ---- | ---- | ------ | ---- |
| env | string | 否 | release、test | 请求环境<br/>release：正式环境（默认）<br/>test：测试环境 |
| space_id  | string | 是 | - | 由P云分配广告位ID |
| openid | string | 是 | - | 合作方用户ID，用于精准投放追踪用户 |
| app_id | string | 是 | - | 由P云分配接入应用ID |
| provider_code | string | 是 | - | 流量主编号 |
| personas | object | 否 | - | 为精准投放传入业务，详见下方说明 |

### personas参数说明

| 变量 | 类型 | 必填 | 可选值 | 说明 |
| ---- | ---- | ---- | ------ | ---- |
| plate | string | 否 | - | 车牌 |
| mobile | string | 否 | - | 手机号 |


## 5.特殊场景说明
- 跳转webview的情况：
    当广告点击后跳转的目标是h5链接的情况下，组件内部会返回一个回调事件(render)，返回跳转的链接H5及投放ID（广告曝光时需要传入）


| 变量 | 类型 | 说明 |
| --- | --- | --- |
| type | string | 跳转的类型，目前只返回webview |
| link | string | 跳转的H5，需要业务方建立一个webview页面承载H5  |
| push_id | string | 需要业务方在webview加载完毕后传入push_id调用曝光接口进行曝光 |


**需要准备的工作：**
1. 支持H5链接的跳转 [webview文档](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html)
2. 在webview加载完成后调用我方的曝光接口

```
// index.wxml
<4pyun-ad-sdk
  // ...
  bindrender="getInfo"
>
</4pyun-ad-sdk>
```

```
// index.js
page({
    methods: {
        getInfo(result) {
            // 需业务方支持webview的加载
            wx.navigateTo({
                url: `/pages/web-view/index?src=${result.link}&push_id=${result.push_id}`
            })
        }
    }
})
```

```
// pages/web-view/index.wxml

<web-view src="{{src}}" bindload="webload"></web-view>
```
```
// pages/web-view/index.js

const adService = require('4pyun-ad-sdk/index.js')

page({
    data: {
        src: "",
        push_id: ""
    },
    onLoad(options) {
        this.setdata({
            src: options.src,
            push_id: options.push_id
        })
    },
    methods: {
        webload(e) {
            if (e.detail.src) {
                // 加载成功，调用曝光
                adService.ack(this.data.push_id, 5);
            }
        }
    }
})
```
