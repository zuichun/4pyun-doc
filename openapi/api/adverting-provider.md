## P云在需要同步流量主信息时候, 请求该接口同步流量主详情。

### 请求参数

| 字段 | 说明| 类型 | 必须 | 示例 |
| --- | --- | --- | --- |
| app_id       | 平台分配的接入应用ID    | string |  Y   | op1234567723122        |
| timestamp  | 当前请求时间戳, 单位毫秒            | string |  Y   |1570318158852                    |
| sign_type  | 签名算法                            | string |  Y   |MD5                              |
| provider_code | 流量主编号 | string | Y | 110 |
| sign         | 请求数据签名  | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39          |

### 响应参数

| 字段 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| code    | string | Y | 业务处理状态码: 1001-处理成功, 1002-流量主不存在, 1500-处理失败 |
| message | string | N | 业务处理状态说明 |
| hint    | string | N | 提示说明 |
| provider_name | string | Y | 流量主名称 |
| address | string | Y | 所在地址, 例如: 广东省深圳市深圳湾科技生态园7B栋905 |
| area_code | string | Y | 城市行政区编码, 例如深圳市南山区为: 440305, 对照: http://www.mca.gov.cn/article/sj/xzqh/1980/201803/201803131454.html |
| longitude | double | N | 所处经度 |
| latitude | double | N | 所处纬度 |
| provider_code | string | N | P云流量主编号, 如果有则返回 |

返回示例:

```json
{
  "address" : "四川省成都市锦xxxx号6栋1单元14楼",
  "provider_name" : "xxxxxx-停车场",
  "area_code" : "510104",
  "longitude" : 104.11436900219999,
  "code" : "1001",
  "message" : "处理成功",
  "latitude" : 30.6407688587
}
```
