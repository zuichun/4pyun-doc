# 推送异常放行记录

### 请求地址

  POST https://api.4pyun.com/gate/1.0/parking/internal/update

### 特殊说明

<b>注意事项</b>

- 同一停车场`flow_no`不能重复, 重复的流水将被忽略!

### 请求参数

| 字段名称   | 字段说明 |  类型  | 必填 | 示例  |
| :--- | :--- | :---: | :--: | :--- |
| app_id  | 平台分配的接入应用ID, 车场本地发起请求可不传递 | string |  N   | op1234567723122|
| sign | 请求数据签名| string |  N   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39  |
| park_uuid  | 平台分配的停车场UUID | string |  Y   | PARKUUID-XXXX-XXX-XXX |
| flow_no | 变更流水号, 相同 | string |  Y   | FLOW000001 |
| parking_serial | 停车场端的停车流水, 一般为入场记录ID | string |  Y   | PARKINGSERIAL-123456789  |
| plate| 车牌号码 | string |  Y  | 粤B12345 |
| plate_color| 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知 | string |  N   | 1  |
| car_type | 车类: 1.临停车辆, 2.月卡车辆, 3.贵宾车辆(免费车), 4.储值车辆, 0.其他未知 | string |  N   | 1  |
| car_desc | 车类描述 | string |  N   | 临停/月卡A  |
| enter_time| 入场时间, 格式: yyyy-MM-dd'T'HH:mm:ss'Z' <br>特别说明UTC时间和普通时间差8小时因为我们在东八区 2022-09-01T00:00:00.000Z 对应时间的时间是 2022-09-01 08:00:00|string|N|2021-09-02T09:36:46.020Z|
| leave_time| 离场时间, 格式: yyyy-MM-dd'T'HH:mm:ss'Z' <br>特别说明UTC时间和普通时间差8小时因为我们在东八区 2022-09-01T00:00:00.000Z 对应时间的时间是 2022-09-01 08:00:00|string|N|2021-09-02T09:36:46.020Z|
| operate_time| 操作时间, 格式: yyyy-MM-dd'T'HH:mm:ss'Z' <br>特别说明UTC时间和普通时间差8小时因为我们在东八区 2022-09-01T00:00:00.000Z 对应时间的时间是 2022-09-01 08:00:00|string|Y|2021-09-02T09:36:46.020Z|
| operator | 操作员 | string |  Y   | 张三 |
| reason | 放行原因 | string |  Y   | 设备异常 |

### 返回结果参数说明
| 字段名称 | 字段说明 |  类型  | 必填 | 备注  |
| :--- | :--- | :---: | :--: | :--- |
| code  | 请求状态码  | string |  Y   | 1001-成功受理<br> 400:参数错误<br> 403:访问被拦截<br>500:服务器内部错误<br>503:服务暂不可用 |
| message  | 返回描述 | string |  Y   | 返回描述 |
| hint  | 返回错误说明   | string |  N   | 返回具体错描述指导|
| seqno | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值|

### 请求示例

```java
final String APP_SECRET = "XXXXXX";

try {
    // 自动对key进行排序
    Map<String, String> request = new TreeMap<>();
    // 停车场商户号
    request.put("park_uuid", "XXXXXX");
    request.put("parking_serial", "2023941102041693796524190");
    request.put("plate", "川A" + RandomUtils.randomAlphanumeric(5).toUpperCase());
    request.put("plate_color", String.valueOf(ColorType.Yellow));
    request.put("flow_no", "UPDATE20230901");
    request.put("car_type", String.valueOf(CarType.Monthly));
    request.put("car_desc", "月卡B");
    request.put("operator", "保安A");
    request.put("reason", "测试测试");

    // 计算签名
    String plain = request.entrySet().stream()
            .map(e -> e.getKey() + "=" + e.getValue())
            .collect(Collectors.joining("&"));
    // 算出MD5值
    String sign = MD5.encryptHEX(plain + "&app_secret=" + APP_SECRET);
    request.put("sign", sign);
    System.out.println("加密明文: " + plain);
    System.out.println("加密结果: " + sign);

    MultipartEntityBuilder requestEntity = MultipartEntityBuilder.create()
            .setCharset(StandardCharsets.UTF_8);
    request.entrySet().forEach(e -> {
        requestEntity.addTextBody(e.getKey(), e.getValue(), ContentType.DEFAULT_TEXT.withCharset(StandardCharsets.UTF_8));
    });

    HttpResponse response = Request.Post("https://api.4pyun.com/gate/1.0/parking/internal/update")
            .body(requestEntity.build())
            .execute()
            .returnResponse();
    String body = EntityUtils.toString(response.getEntity());
    /**
     * 1. HTTP非200也会返回结果, 故需读取BODY!
     * 2. 该接口返回code取值200、1001、1000均表示上传成功!
     */
    // 打印返回结果
    System.out.println(body);
} catch (IOException e) {
    e.printStackTrace();
}
```

### 返回结果示例



```
正常返回
{
  "code": "1001",
  "message": "OK",
  "seqno": "abc6c253b93cc940",
  "data_node": "CN-South/HS3-1"
}
```

```
{
    "code":"401",
    "message":"请求签名校验不通过",
    "hint":"park_uuid=49f0cc52-e8c7-41e3-b54d-af666b8cc11a&parking_serial=parking_serialxxxxx&plate=?KKKKKK&release_time=2023-03-21T15:59:59Z&app_secret=***",
    "seqno":"17224239894628849085663174788803",
    "data_node":"CN-South/DEV-1",
    "path":"POST /gate/1.0/parking/internal/release"
}
```
