# 云端HTTP接入指引

若贵公司已有对外的HTTP协议接口，请联系平台确认对接方式；若无则直接按照平台的标准直接接入。

参考项目
```
https://github.com/4PYun/4pyun-api-http
```

实现起来也很简单，协议规则和本地接入一摸一样，你提供一个接口地址即可，例如

```
http://127.0.0.1:8080/gateway/1.0/dispatch
```

那么获取订单的时候会将请求参数按照协议规范序列化成JSON后POST到上面提供的接口网关地址。

**请求示例:**
```bash
POST http://127.0.0.1:8080/gateway/1.0/dispatch
Content-Type: applicaiton/json

{
  "service" : "service.parking.payment.billing",
  "charset" : "UTF-8",
  "plate" : "粤TSXXXXX",
  "version" : "1.0",
  "park_uuid" : "UIDDDDDDDD123",
  "sign" : "612BB8F6B7BFF82F60B04AEB8955E156",
  "gate_id" : ""
}
```

**返回示例:**
```json
{
  "parking_serial" : "PARK749582826155802625",
  "enter_free_time" : 60,
  "enter_time" : "20220831185429",
  "message" : "",
  "parking_time" : 6511,
  "autopay_status" : 0,
  "buffer_time" : 1800,
  "park_uuid" : "UID210913092606489001",
  "plate" : "粤TSXXXXX",
  "total_value" : 0,
  "pay_value" : 0,
  "charset" : "UTF-8",
  "free_value" : 0,
  "locking_status" : 0,
  "parking_order" : "PAY749582826155802627",
  "park_name" : "城市便捷酒店停车场",
  "service" : "service.parking.payment.billing",
  "result_code" : 1001,
  "paid_value" : 0
}
```
