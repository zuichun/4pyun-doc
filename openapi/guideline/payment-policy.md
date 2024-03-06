# 支付接入要求

```
2024-01-01
```

**尊敬的P云战略合作伙伴:**

为响应政府的号召，未按照政府要求接入数据的车场，我司将在2024年4月1日关闭支付权限，届时会导致车场无法支付。为避免给您造成不便，恳请合作伙伴们及时按照以下接口接入数据：

|事项|接入要求|相关接口|
|---|---|---|
|下单传递交易场景信息|需接入|[被扫请求](./../api/trade-authpay.html)<br/>[交易预请求](./../api/trade-prepare.html)|
|上报停车记录|需接入|[车辆入场](./../../parking/api/parking-enter.html)<br/>[车辆离场](./../../parking/api/parking-leave.html)<br/>[车场信息查询](./../../parking/api/parking-realtime.html)|
