# 商户优惠券状态核销

## 概述

此接口用于**车场**核销**已使用**的优惠券使用。

## 接口定义

接口地址值： ```https://api.4pyun.com/gate/1.0/parking/internal/mcoupon/grant/sync```

请求方式：```HTTP FORM 表单提交```

使用方式：P云实现，由需求方调用。

## 请求

### 参数

|     参数     |  类型   | 必须 |                             描述                             |              示例值              |
| :----------: | :-----: | :--: | :----------------------------------------------------------: | :------------------------------: |
|     sign     | string  |  Y   |                         请求数据签名                         | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
|  local_sync  |   int   |  N   |                 是否本地派发流水（1:是0:否）                 |                0                 |
|  park_uuid   | string  |  Y   |                          停车场UUID                          |                                  |
| grant_serial | string  |  Y   | local_sync =1时：为本地派发流水 local_sync =0时：平台下发的派发流水 |                                  |
|    status    |   int   |  Y   |                      派发状态：固定传1                       |                1                 |
| status_time  | utc时间 |  N   |                         默认当前时间                         |     2022-02-28T03:30:28.000Z     |
| apply_status |   int   |  Y   |                      核销状态：固定传1                       |                1                 |
| coupon_value |   int   |  Y   |   实际优惠面额(时长券和金额券单位都是分,非时间金额券的传0    |                                  |
| apply_value  |   int   |  Y   |                       实际优惠金额,分                        |               100                |
|  apply_time  | utc时间 |  N   |               核销时间, 默认取当前收到请求时间               |     2022-02-28T03:30:28.000Z     |

## 特殊说明

UTC时间：UTC时间与中国标准时间（CST）相差8小时

### 示例

```json
{
    "sign": "44CAF15ED1CDED9D5072F83A9997775F",
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "grant_serial":"6020231117153244nullnull00002",
    "status":1,
    "status_time":"2023-11-17T07:32:49Z",
    "apply_status":1,
    "coupon_value":0,
    "apply_value":100,
    "apply_time":"2023-11-17T07:32:49Z"
}
```



## 响应

### 参数

|  参数   |  类型  | 必须 |                     描述                     |  示例值  |
| :-----: | :----: | :--: | :------------------------------------------: | :------: |
|  code   | string |  Y   |             1001：成功 其它失败              |   1001   |
| message | string |  Y   |                   返回描述                   | 核销成功 |
|  hint   | string |  N   |                 返回错误说明                 |          |
|  seqno  | string |  Y   | 服务器日志标示查日志用到查问题尽量提供这个值 |          |

### 示例

```json
{
    "code": "1001",
    "message": "核销成功",
    "seqno": "05279267317675712045263067083697",
    "data_node": "dev/DEV-3",
    "time_cost": 23
}
```
