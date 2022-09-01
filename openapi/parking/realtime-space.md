# 获取实时车位

### 请求方式

`GET` `gate/1.0/parking/realtime/space`

### 请求

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| park_uuid | string | Y | 平台停车场编号 | 1 |

#### 示例

```
https://api.4pyun.com/gate/1.0/parking/realtime/space?app_id=op00961963581daa7&timestamp=1628087581124&sign=3A8A2161B6D46873FB35EC1683C7B69A&park_uuid=631edbdb-b2c2-4752-b12b-4bc3101aa0e2
```

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| merchant | string | Y | 平台停车场商户号 | 62626601 |
| park_uuid | string | Y | 平台停车场编号 | 49f0cc52-e8c7-41e3-b54d-af666b8cc11a |
| park_name | string | Y | 平台停车场名称 | P云支付体验-停车场 |
| total_space | int | Y | 总车位数 | 0 |
| remain_space | int | Y | 空余车位数 | 0 |
| mode | int | Y | 模式 | 0 |
| remark | string | Y | 备注 |  |
| total_reserve_space | int | Y | 开放预约车位数 | 0 |
| remain_reserve_space | int | Y | 开放预约剩余车位数 | 0 |
| area_space | object[] | Y | 区域车位信息 |  |

**区域信息：`area_space`**
| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| area_name | string | Y | 区域名称 | 地面 |
| total_space | int | Y | 区域总车位数 | 0 |
| remain_space | int | Y | 区域空余车位数 | 0 |

#### 示例

```
{
  "code": "1001",
  "seqno": "f659c30f4ff37e8c",
  "data_node": "CN-South/HS3-2",
  "time_cost": 35,
  "payload": {
    "merchant": "42502219",
    "park_uuid": "631edbdb-b2c2-4752-b12b-4bc3101aa0e2",
    "park_name": "深圳图书馆-停车场",
    "total_space": 238,
    "remain_space": 118,
    "mode": 0,
    "remark": "",
    "total_reserve_space": 0,
    "remain_reserve_space": 0,
    "area_space": [
      {
        "area_name": "地面停车场",
        "total_space": 70,
        "remain_space": 36
      },
      {
        "area_name": "地库停车场",
        "total_space": 168,
        "remain_space": 82
      }
    ]
  }
}
```
