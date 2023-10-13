# 离场记录推送


**注意：推送必须基于有效的车牌白名单。**

### 请求方式

`HTTP POST FORM 表单提交`

### 请求

#### 参数

| 参数           | 类型   | 必须 | 描述                                                                                                                                         | 示例值 |
| -------------- | ------ | ---- |--------------------------------------------------------------------------------------------------------------------------------------------| ------ |
| park_uuid      | string | Y    | 停车场编号                                                                                                                                      |        |
| park_name      | string | Y    | 停车场名称                                                                                                                                      |        |
| address        | string | Y    | 停车场地址                                                                                                                                      |        |
| area_code      | string | Y    | 区域编码                                                                                                                                       |        |
| latitude       | string | Y    | 纬度（WGS84）                                                                                                                                  |        |
| longitude      | string | Y    | 经度（WGS84）                                                                                                                                  |        |
| parking_serial | string | Y    | 平台停车流水                                                                                                                                     |        |
| plate          | string | Y    | 车牌号                                                                                                                                        |        |
| plate_color    | string | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#color_type">车牌颜色</a>                                                                  ||
| car_type       | string | Y    | <a href="https://doc.4pyun.com/openapi/appendix.html#car_type">车类</a>||
| car_desc       | string | N    | 车类描述                                                                                                                                       |        |
| enter_time     | string | Y    | 入场时间戳，毫秒                                                                                                                                   |        |
| enter_image    | string | N    | 入场图片地址                                                                                                                                     |        |
| enter_gate     | string | N    | 入场通道编号                                                                                                                                     |        |
| enter_security | string | N    | 入口管理员                                                                                                                                      |        |
| vehicle_type   | string | N    | <a href="https://doc.4pyun.com/openapi/appendix.html#vehicle_type">车型</a>||
| leave_time     | string | Y    | 出场时间戳，毫秒                                                                                                                                   |        |
| leave_image    | string | N    | 出场图片地址                                                                                                                                     |        |
| leave_gate     | string | N    | 出场通道编号                                                                                                                                     |        |
| leave_security | string | N    | 出口管理员                                                                                                                                      |        |
| parking_time   | string | Y    | 停车时长，秒                                                                                                                                     |        |
| total_value    | string | Y    | 总金额，分                                                                                                                                      |        |
| free_value     | string | N    | 优惠金额，分                                                                                                                                     |        |
| autopay_value  | string | N    | 无感支付的金额，分                                                                                                                                  |        |
| total_space  | string | Y | 总车位                                                                                                                                        |        |
| remain_space | string | Y | 剩余车位                                                                                                                                       |        |
| ...            |        |      |                                                                                                                                            ||

#### 示例

```json
{
    "total_value": "100",
    "latitude": "-1.0",
    "sign": "4E26779B1B6D8F940B9EA0A612F1123A",
    "leave_image": "https://660pp.com/img/env_1.jpg",
    "plate": "川A660P1",
    "enter_image": "https://660pp.com/img/env_1.jpg",
    "park_name": "P云支付体验-停车场",
    "car_type": "1",
    "app_id": "op00961963581daa7",
    "sign_type": "MD5",
    "car_desc": "",
    "timestamp": "1618994590324",
    "longitude": "-1.0",
    "parking_time": "305",
    "enter_gate": "入口",
    "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
    "leave_time": "1618994585000",
    "leave_gate": "出口",
    "address": "广东省深圳市南山区粤海街道讯美科技广场3号楼",
    "plate_color": "-1",
    "area_code": "440305",
    "autopay_value": "0",
    "parking_serial": "591668151763415041",
    "vehicle_type": "0",
    "enter_time": "1618994280000",
    "free_value": "0"
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
