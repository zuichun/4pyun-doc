# 同步设备

### 接口描述

1. 同步设备信息至平台；
2. 设备状态变更时同步至平台等。

### 接口地址

测试环境：`https://dev-api.4pyun.com/gate/1.0/energy/internal/device/sync`
</br>
生产环境：`https://api.4pyun.com/gate/1.0/energy/internal/device/sync`

### 请求方式：`POST`
### 数据类型：`application/json; charset=utf-8`
### 请求头

| 参数名称          | 说明  |
|---------------|-----|
| Authorization | 签名值 |

### 请求参数

| 参数名称         | 类型     | 是否必须 | 说明                                                                          |
|--------------|--------|------|-----------------------------------------------------------------------------|
| app_id       | string | Y    | P云平台分配的应用标识                                                                 |
| station_uuid | string | Y    | P云平台创建的充电站编号                                                                |
| no           | string | Y    | 本地设备编号                                                                      |
| state        | int    | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#device_state">设备状态</a> |
| state_desc   | string | Y    | 设备状态描述                                                                      |
| energy_code  | string | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#energy_code">设备类型</a>  |
| port         | object | Y    | 枪口数据                                                                        |

`port`

| 参数名称         | 类型     | 是否必须 | 说明                                                                          |
|--------------|--------|------|-----------------------------------------------------------------------------|
| no           | string | Y    | 本地枪口编号。例如：DA02M01                                                           |
| state        | int    | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#port_state">枪口状态</a>   |
| state_desc   | string | Y    | 枪口状态描述                                                                      |
| alias        | string | Y    | 枪口别名。例如：A                                                                   |
| space_status | int    | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#space_status">车位状态</a> |

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
// 请求参数
TreeMap<String, Object> args = new TreeMap<String, Object>() {{
    put("app_id", APP_ID);
    put("station_uuid", "8f5fdb60-9374-4c11-bdc2-a32d8369258c");
    put("no", "D012026");
    put("state", 1);
    put("state_desc", "在线");
    put("energy_code", "CN_AC");
    put("port", new ArrayList<HashMap<String, Object>>() {{
        add(new HashMap<String, Object>() {{
            put("no", "D01202601");
            put("state", StationPortState.AVAILABLE);
            put("state_desc", "空闲");
            put("space_status", -1);
            put("alias", "A");
        }});
        add(new HashMap<String, Object>() {{
            put("no", "D01202602");
            put("state", StationPortState.BUSY);
            put("state_desc", "充电中");
            put("space_status", 2);
            put("alias", "B");
        }});
    }});
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
