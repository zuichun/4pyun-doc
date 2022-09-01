# 推送车辆无感状态

**注意：推送必须基于有效的车牌白名单。**

### 请求方式

`POST`

### 请求

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例值 |
|-|-|-|-|-|
| park_uuid      | string | Y    | 停车场编号       |        |
| park_name      | string | Y    | 停车场名称       |        |
| plate          | string | Y    | 车牌号           |        |
| plate_color    | string | Y    | 车牌颜色，参考：https://pptingche.coding.net/s/7d8bdcd9-636e-4227-8f29-0eaac3c4eae8 ||
| autopay_channel       | string | N    | 无感支付渠道。为空则表示无感关闭 ||
| autopay_status       | string | Y | 无感状态。1：开通；0：关闭 |        |

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "park_name": "P云支付体验-停车场",
    "plate": "川A660P1",
    "plate_color": "-1",
    "autopay_channel": "300002",
    "autopay_status": "1",
    "sign": "8E178615183E9C0123BE254A1730EED6"
}
```

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| code | string | Y | 状态码 | 1001 |

#### 示例

```json
{
    "code": "1001"
}
```

### 附录

#### 无感渠道
|渠道编号|描述|
|-|-|
|300004|支付宝车主平台|
|300005|微信车主平台|
