# 同步充电记录

### 接口描述

1. 充电过程中定时上报充电进度；
2. 充电完成上报充电账单；
3. 充电启动失败上传记录；
4. 也可用于充电减免停车费场景。充电完成上传充电记录（车牌必须），平台会匹配充电站对应停车场的优惠规则进行下发。

### 接口地址

- 测试环境：`https://dev-api.4pyun.com/gate/1.0/energy/internal/replenish/sync`
- 生产环境：`https://api.4pyun.com/gate/1.0/energy/internal/replenish/sync`

### 请求方式：`POST`
### 数据类型：`application/json; charset=utf-8`
### 请求头

| 参数名称          | 说明  |
|---------------|-----|
| Authorization | 签名值 |

### 请求参数

| 参数名称         | 类型     | 是否必须 | 说明                                                                             |
|--------------|--------|------|--------------------------------------------------------------------------------|
| app_id       | string | Y    | P云平台分配的应用标识                                                                    |
| station_uuid | string | Y    | P云平台创建的充电站编号                                                                   |
| order        | string | Y    | 本地充电单号                                                                         |
| start_time   | string | Y    | 充电开始时间，UTC时间。例如：`2024-04-14T16:00:00.000Z` 表示 `2024-04-15 00:00:00`            |
| end_time     | string | Y    | 充电开始时间，UTC时间。例如：`2024-04-14T16:00:00.000Z` 表示 `2024-04-15 00:00:00`            |
| vin          | string | N    | 车辆识别码                                                                          |
| plate        | string | N    | 车牌号码。充电减免停车费场景下必须传递                                                            |
| quantity     | int    | Y    | 充电量，单位：0.001Kwh，1表示0.001Kwh。例如1度则传递1000                                        |
| energy_value | int    | Y    | 充电电费，单位：分                                                                      |
| fee_value    | int    | Y    | 充电服务费，单位：分                                                                     |
| state        | int    | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#replenish_state">充电状态</a> |
| state_desc   | string | Y    | 充电状态描述                                                                         |
| device_no    | string | Y    | 本地设备编号                                                                         |
| device_type  | int    | N    | <a href="https://doc.4pyun.com/openapi/appendix.html#device_type">设备类型</a>，默认0 |
| port_no      | string | Y    | 本地枪口编号                                                                         |
| energy_code  | string | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#energy_code">充电类型</a>     |
| soc          | int    | N    | SOC，单位：百分比。例如100则表示百分百                                                         |
| mobile       | string | Y    | 手机号码，未收集到时可传递本地用户标识                                                            |

### 响应参数
| 字段名称    | 字段说明   |   类型   | 必填 | 备注              |
|:--------|:-------|:------:|:--:|:----------------|
| code    | 状态码    | string | Y  | 1001            |
| message | 错误描述   | string | Y  | 错误描述            |
| hint    | 错误说明提示 | string | Y  | 返回具体错描述指导       |
| seqno   | 请求序列号  | string | Y  | 查日志用到查问题尽量提供这个值 |

### 响应码
| 值    | 描述                         |
|------|----------------------------|
| 401  | APPID或签名错误，                |
| 400  | 请求参数错误                     |
| 1001 | 成功                         |
| 1500 | 失败                         |
| 其它   | 参考 `message` 内容或 `hint` 提示 |


### 示例代码

```java
SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'");
format.setTimeZone(TimeZone.getTimeZone("UTC"));

// 请求参数
TreeMap<String, Object> args = new TreeMap<String, Object>() {{
    put("app_id", APP_ID);
    put("station_uuid", "8f5fdb60-9374-4c11-bdc2-a32d8369258c");
    put("order", "202404111434431656839748");
    put("start_time", format.format(DateTime.now().minusHours(1).toDate()));
    put("end_time", format.format(new Date()));
    put("vin", "");
    put("plate", "川A660PP1");
    put("quantity", 1000);
    put("energy_value", 1);
    put("fee_value", 1);
    put("state", 3);
    put("state_desc", "充电完成");
    put("device_no", "D012026");
    put("device_tyoe", 0);
    put("port_no", "D01202601");
    put("energy_code", "CN_AC");
    put("soc", 100);
    put("mobile", "13800138000");
}};

// 计算签名
String content = JSON.toJSONString(args);
String plain = content + "&app_secret=" + APP_SECRET;
String sign = MD5.encryptHEX(plain);

// 请求接口
HttpResponse response = Request.Post("https://api.4pyun.com/gate/1.0/energy/internal/replenish/sync")
        .addHeader("Authorization", sign)
        .addHeader("Content-Type", "application/json; charset=utf-8")
        .bodyString(content, ContentType.APPLICATION_JSON)
        .execute()
        .returnResponse();
// 注意：非200也会返回响应体
System.out.println(response.getStatusLine());
System.out.println(EntityUtils.toString(response.getEntity()));
```

### 签名规则

1. 将密钥以 `&` 为分隔符添加到请求体中JSON字符串后面，生成待签名字符串；
2. 使用标准MD5算法对待签名字符串进行签名；
3. 将签名值写入请求头的 `Authorization` 字段。

例如：
<br>
待签名字符串：`{"a":"string","b":0,"c":1900000109}&app_secret=您的密钥`
<br>
正确签名：`d7f3eca20c666483b2f4963d35a3f547`
