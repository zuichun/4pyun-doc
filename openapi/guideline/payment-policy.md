# 支付接入要求

```
2024-01-01
```

**尊敬的P云战略合作伙伴:**

根据停车支付业务监管要求，我司将在`2024年4月1日`关停未按照接入标准交易商户，届时会导致商户无法发起支付。

为避免给您造成不便，请您及时按照以下支付接口接入：

|事项|接入要求|相关接口|
|---|---|---|
|下单传递交易场景信息|需接入|[被扫请求](./../api/trade-authpay.html)<br/>[交易预请求](./../api/trade-prepare.html)|
|上报停车记录|需接入|[车辆入场](./../../parking/api/parking-enter.html)<br/>[车辆离场](./../../parking/api/parking-leave.html)<br/>[车场信息查询](./../../parking/api/parking-realtime.html)|
