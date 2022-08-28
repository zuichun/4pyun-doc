## JS SDK

### 1. 引入SDK
```html
<script src="https://assets.4pyun.com/js/4pyun-sdk-1.0.0.min.js"></script>
```

### 2. 设置广告节点

```html
<div id="image-banner"></div>
```

### 3. 渲染广告
```js
pyun.advertingPull({
    el: 'image-banner', // 必填 节点id
    app_id: '由P云分配接入应用ID', //
    openid: '合作方用户ID，用于精准投放追踪用户', // 选填
    space_id: '由P云分配广告位ID', // 广告位必填
    displayed: 1, // 必填
    provider_code: '流量主编号', // 项目流量主编号, 兼容P云或者客户内部项目编号
    external: 1, // 外部流量主标志: 0 否, 1是(仅当provider_code非P云提供设置为1)
    personas: { // 选填
        // 为精准投放传入业务
        plate: '粤BAAAAAA', // 车牌信息 至少包含前两位
    },
    success: (data) => {
        // 广告加载成功回调
        console.log(data);
    },
    failure: (cause) => {
        // 广告加载失败回调
        console.warn(cause);
    },
});
```

### 4. 手动渲染广告

SDK默认直接渲染广告节点，若需要控制渲染，例如采用弹窗的的形式可通过以下方式控制:

```html
<div id="banner-warpper" style="display:none">
    <div id="image-banner"></div>
</div>
```

```js
pyun.advertingPull({
    el: 'image-banner', // 必填 节点id
    app_id: '由P云分配接入应用ID', //
    openid: '合作方用户ID，用于精准投放追踪用户', // 选填
    space_id: '由P云分配广告位ID', // 广告位必填
    displayed: 1, // 必填
    provider_code: '流量主编号', // 项目流量主编号, 兼容P云或者客户内部项目编号
    external: 1, // 外部流量主标志: 0 否, 1是(仅当provider_code非P云提供设置为1)
    personas: { // 选填
        // 为精准投放传入业务
        plate: '粤BAAAAAA', // 车牌信息 至少包含前两位
    },
    success: (data) => {
        // 广告加载成功回调
        // 弹窗代码
        // ...
        document.getElementById('banner-warpper').style.display = 'block';
    },
    failure: (cause) => {
        // 广告加载失败回调
        console.warn(cause);
    },
});
```


### 测试参数
```js
app_id=op00961963581daa7
```
