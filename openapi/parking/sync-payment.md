# 临停支付订单推送

**注意：推送必须基于有效的车牌白名单。**

### 请求方式

`POST`

### 请求

#### 参数

| 参数           | 类型   | 必须 | 描述             | 示例值 |
| -------------- | ------ | ---- | ---------------- | ------ |
| park_uuid      | string | Y    | 停车场编号       |        |
| park_name      | string | Y    | 停车场名称       |        |
| parking_serial | string | Y    | 平台停车流水     |        |
| pay_serial     | string | Y    | 平台支付流水     |        |
| pay_value      | string | Y    | 支付金额，分     |        |
| pay_time       | string | Y    | 支付时间戳，毫秒 |        |
| plate          | string | Y    | 车牌             |        |
| channel        | string | Y    | 支付渠道         |        |
| identity       | string | Y    | 平台用户标识     |        |
| discounts      | []     | N | 当前订单使用折扣信息         |        |
| mcoupons | [] | N | 停车商户优惠信息 | |
| ...            |        |      |                  |        |

`discounts`

| 参数          | 类型   | 必须 | 描述         | 示例值 |
| ------------- | ------ | ---- | ------------ | ------ |
| type          | string | Y    | 优惠类型     |        |
| token         | string | Y    | 优惠凭证令牌 |        |
| openid        | string | Y    | 用户标识     |        |
| mobile        | string | Y    | 手机号       |        |
| relief_amount | string | Y    | 抵扣数量     |        |
| relief_value  | string | Y    | 抵扣金额，分 |        |
| ...           |        |      |              |        |

`mcoupons`

| 参数 | 类型 | 必须 | 描述 | 示例值 |
| - | - | - | - | - |
| coupon_name | string | Y | 优惠券名称 | 10元停车券 |
| coupon_code | string | Y | 优惠券编号 | 0x01 |
| store_name | string | Y | 商家名称 | 肯德基 |
| store_code | string | Y | 商家编号 | 0x02 |
| coupon_type | string | Y | 优惠类型, 参考: com.chinaroad.api.v1.parking.DiscountType | 1 |
| coupon_value | string | Y | 优惠面额 | 1000 |
| grant_time | string | Y | 派发时间 |  |
| grant_serial | string | Y | 派发流水 |  |

`DiscountType`
| 值 | 描述 |
| - | - |
| 1 | 金额优惠（单位：分） |
| 2 | 时间优惠 |
| 3 | 全免类型优惠 |
| 4 | 不同计价优惠 |
| 5 | 长期有效券 |
| 6 | 折扣券 |


*注意：优惠券只包含第三方优惠券，P云平台券，微信立减金等银行渠道优惠不会推送。*    

#### 示例

```json
{
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "pay_serial": "20210421164242075523115813",
    "discounts": "[]",
    "pay_value": "100",
    "sign": "B1BAA9B6C83DB3E0C50F84B7B54AC848",
    "parking_serial": "591668151763415041",
    "app_id": "op00961963581daa7",
    "sign_type": "MD5",
    "park_name": "P云支付体验-停车场",
    "timestamp": "1618994562891",
    "pay_time": "1618994563000",
    "plate": "川A660P1",
    "channel": "200102",
    "identity": "5aeV05nlt6/ig4LplITisYg="
}
```

### 响应

#### 参数

| 参数 | 类型 | 必须 | 描述 | 示例 |
|-|-|-|-|-|
| code | string | Y | 状态码 | 001 |

#### 示例

```
{
    "code": "1001"
}
```
