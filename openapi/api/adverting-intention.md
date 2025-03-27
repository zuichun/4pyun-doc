# 修改广告推广意向

```
POST https://api.4pyun.com/gate/1.0/adverting/advertising/demand/intention
```

**接口说明**

	修改推广需求下属关联的所有投放广告的曝光定向

**请求参数**

| 字段名称 | 字段说明 |  类型  | 必填 | 示例  |
| :--- | :--- | :---: | :---: | :--- |
| app_id | 平台分配的接入应用ID | string |  Y | op1234567723122 |
| demand_id | 平台提供的推广需求ID | string |  Y | 77896521112  |
| intention  | 业务参数 | Array |  Y | 参见intention参数示例 |

​ intention

| 字段名称 | 字段说明 |  类型  | 必填 | 示例  |
| :--- | :--- | :---: | :---: | :--- |
| key | 定向KEY | string |  Y | 参见intention.key-value参数值说明 |
| value | 定向值 | string |  Y | 参见intention.key-value参数值说明  |
| reverse  | 类型 | int |  Y | 0：选中 1：过滤 |

​ intention.key-value参数值说明

| key | key值说明 | value | value值说明 |
| :---: | :--- | :---: | :--- |
| ip_location | IP归属地 | 广东省:440000<br>广东省深圳市:440000,440300<br>广东省深圳市南山区:440000,440300,440305 | 省市区行政编码（省市区之间用 , 分隔） |

**请求示例**

​ 广告定向投放IP归属地: 广东省深圳市南山区和广西省
```json
{"app_id":"op1234567723122","demand_id":77896521112,"intention":[{"key":"area_code","value":"440000,440300,440305","reverse":0},{"key":"area_code","value":"450000","reverse":0}]}
```

​ 广告定向过滤IP归属地: 广西省
```json
{"app_id":"op1234567723122","demand_id":77896521112,"intention":[{"key":"area_code","value":"450000","reverse":1}]}
```

**请求返回结果参数说明**

| 字段名称   | 字段说明     |  类型  | 必填 | 备注   |
| :--- | :--- | :---: | :---: | :--- |
| code | 请求状态码 | string | Y | 1001-成功<br>其它-读取message |
| message | 返回描述 | string | N | 返回描述 |
| hint | 返回错误说明 | string | N | 返回具体错误描述指导 |
| seqno | 服务器日志标示 | string | Y | 服务器日志标示编码 |

**请求返回结果示例:**

​ 请求成功

```json
{
    "code": "1001",
    "seqno": "64369191506987127961410787347922",
    "data_node": "CN-South/DEV-3",
    "time_cost": 61
}
```

​ 请求失败

```json
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "检查请求参数`app_id`!",
    "seqno": "84735261540785178817961040998334",
    "data_node": "CN-South/DEV-3",
    "path": "POST /gate/1.0/adverting/advertising/demand/intention"
}
```