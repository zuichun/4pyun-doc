# 充电记录推送

### 请求地址

```
https://api.4pyun.com/gate/1.0/energy/internal/replenish
```

### 调用方式

```
HTTP FORM 表单提交
```

### 特殊说明

**注意事项：**

1. 需在充电结束时触发调用；
2. 一次充电单号唯一；
3. 下发停车优惠时 `vin` 需上传车牌；

**参数说明：**

1. `appId` 和 `appSecret` 由P云平台分配；
2. `station_uuid` 为P云平台 `充电服务商` 板块充电站的编号；


### 请求参数

| 参数 | 类型 | 必须 | 描述 | 示例值 |
|-|-|-|-|-|
| app_id | string |  Y | 平台分配的接入应用ID | op1234567723122 |
| timestamp | string |  Y | 请求发送时间戳(ms), 用于防重放攻击服务器直接受和服务器时间差10分钟内的请求。 | 1552976318722 |
| sign | string | Y | 请求数据签名 | C65FCAC2D3FB5E2D3D4AD93DD20C8C39  |
| station_uuid      | string | Y    | 平台充电站编号       | 9b811f5e-df65-46a0-bec6-b56afbbbf8ea |
| device_no      | string | Y    | 本地设备编号       | DEVICE1 |
| port_no        | string | Y    | 本地设备枪号       | 1 |
| replenish_order      | string | Y    | 本地充电单号  |        |
| start_time | string | Y    | 开始充电，`yyyy-MM-dd'T'HH:mm:ss'Z'` | 2023-04-10T17:37:30Z |
| end_time | string | Y | 结束充电，`yyyy-MM-dd'T'HH:mm:ss'Z'` | 2023-04-10T17:37:30Z |
| vin | string | N   | 车辆标识。充电停车优惠场景下该值必须传递停车记录的车牌 | 川A660PP |
| quantity | string | Y    | 充电度数，0.001Kwh，1表示0.001Kwh，例如1度传递1000即可 | 1000 |
| energy_value | string | Y    | 电费，分       | 1 |
| fee_value | string | Y    | 服务费，分 |1|
| total_value | string | Y | 总金额，分 |2|
| energy_code | string | Y    | 充电类型；CN_AC：慢充；CN_DC：快充 |CN_AC|
| ...            |        |      |                  |        |

### 请求示例

```json
{
    "fee_value": 561,
    "total_value": 1156,
    "quantity": "5682",
    "replenish_order": "20230410183256K7fh6t",
    "end_time": "2023-04-10T18:32:56Z",
    "sign": "4EC351C604ECB191964EB67565AA8E87",
    "device_no": "S1",
    "energy_code": "CN_AC",
    "start_time": "2023-04-10T17:32:56Z",
    "energy_value": 595,
    "vin": "川A660N2",
    "station_uuid": "8f5fdb60-9374-4c11-bdc2-a32d8369258c",
    "app_id": "op00961963581daa7",
    "timestamp": "1681122776000",
    "port_no": "1"
}
```

### 响应参数
| 字段名称 | 字段说明 |  类型  | 必填 | 备注  |
| :--- | :--- | :---: | :--: | :--- |
| code  | 请求状态码  | string |  Y   | 200:成功受理<br> 400:参数错误<br> 403:访问被拦截<br>500:服务器内部错误<br>503:服务暂不可用 |
| message  | 返回描述 | string |  Y   | 返回描述 |
| hint  | 返回错误说明   | string |  N   | 返回具体错描述指导|
| seqno | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值|


### 响应示例

> 正常返回

```json
{
    "code": "1001",
    "seqno": "32632654459897019054641535481549",
    "data_node": "CN-South/DEV-1",
    "time_cost": 148
}
```

> 签名错误。参考 `hint`

```json
{
    "code": "401",
    "message": "请求签名校验不通过",
    "hint": "app_id=op00961963581daa7&device_no=S1&end_time=2023-04-11T14:07:39Z&energy_code=CN_AC&energy_value=310&fee_value=102&port_no=1&quantity=1003&replenish_order=20230411140739AD7ln7&start_time=2023-04-11T13:07:39Z&station_uuid=8f5fdb60-9374-4c11-bdc2-a32d8369258c&timestamp=1681193259128&total_value=412&vin=川A660N2&app_secret=***",
    "seqno": "02118331862997565649094193731652",
    "data_node": "CN-South/DEV-1",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 参数错误。参考 `hint`

```json
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "`device_no` required~",
    "seqno": "01645210617331334902890840957848",
    "data_node": "CN-South/DEV-1",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 服务器内部错误

```json
{
    "code": "500",
    "message": "服务器内部错误",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

> 服务器内部错误

```json
{
    "code": "503",
    "message": "服务暂不可用",
    "seqno": "48bf04ea4caa479c",
    "data_node": "CN-South/HS3-2",
    "path": "POST /gate/1.0/energy/internal/replenish"
}
```

### 示例代码

```java
@Test
public void execute() throws APIRemoteException {
    // 平台分配的密钥
    String appId = "op00961963581daa7";
    String appSecret = "6409292d66625a2a0912acfc61ed956c";
    // 接口地址
    String url = "https://dev-api.4pyun.com/gate/1.0/energy/internal/replenish";

    // 请求参数
    int energyValue = Integer.parseInt(RandomUtils.randomNumeric(3));
    int feeValue = Integer.parseInt(RandomUtils.randomNumeric(3));
    TreeMap<String, String> params = new TreeMap<String, String>() {{
        put("app_id", appId);
        put("timestamp", String.valueOf(System.currentTimeMillis()));

        put("station_uuid", "8f5fdb60-9374-4c11-bdc2-a32d8369258c");
        put("device_no", "S1");
        put("port_no", "1");
        put("energy_code", "CN_AC");
        put("replenish_order", DateUtils.format(new Date(), DateUtils.DEFAULT_TIME_FORMAT) + RandomUtils.randomAlphanumeric(6));
        put("start_time", DateUtils.format(DateTime.now().minusHours(1).toDate(), DateUtils.ISO8601_DATETIME));
        put("end_time", DateUtils.format(new Date(), DateUtils.ISO8601_DATETIME));
        put("vin", "川A660N2");
        put("quantity", RandomUtils.randomNumeric(4));
        put("energy_value", String.valueOf(energyValue));
        put("fee_value", String.valueOf(feeValue));
        put("total_value", String.valueOf(energyValue + feeValue));
    }};

    /*
     * 计算签名
     * S1: 先过滤掉 sign 和值为空的参数；
     * S2: 在将所有业务参数按照参数名进行升序排序；
     * S3: 以 `HTTP URL` 风格拼接成待签名的字符串，如：`a=1&b=2&c=123`
     * S4: 再将密钥以 `&app_secret=您的密钥` 形式拼接到上一步待签名的字符串后面，如：`a=1&b=2&c=123&app_secret=您的密钥`
     * S5: 使用标准 MD5 算法对上述字符串进行签名（忽略大小写）；
     * S6: 将其写入 `sign` 进行 `HTTP POST` 请求即可。
     */
    String plain = params.entrySet()
            .stream()
            .filter(e -> StringUtils.isNotBlank(e.getValue()))
            .filter(e -> !"sign".equals(e.getKey()))
            .map(e -> e.getKey() + "=" + e.getValue())
            .collect(Collectors.joining("&"));
    plain += "&app_secret=" + appSecret;
    String sign = MD5.encryptHEX(plain);
    params.put("sign", sign);
    // plain: app_id=op00961963581daa7&device_no=S1&end_time=2023-04-11T09:20:00Z&energy_code=CN_AC&energy_value=207&fee_value=975&port_no=1&quantity=6556&replenish_order=202304110920004SfjdX&start_time=2023-04-11T08:20:00Z&station_uuid=8f5fdb60-9374-4c11-bdc2-a32d8369258c&timestamp=1681176000816&total_value=1182&vin=川A660N2&app_secret=6409292d66625a2a0912acfc61ed956c, sign: 90A80901298B87DC9E15DE9F236FD164
    log.info("plain: {}, sign: {}", plain, sign);

    // HTTP 请求
    this.doPost(url, params);
}
```
