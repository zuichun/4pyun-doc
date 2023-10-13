# 附录

## I. 交易场景

|场景值|说明|
|---|---|
|PARKING|停车缴费(临停续费)|
|RECHARGE|停车缴费(固定车续费)|
|ENERGY|充电业务|
|LIVING|生活缴费|
|GENERIC|通用缴费|

### I.I 停车缴费
```
trade_scene=PARKING 场景填写到extra字段
```
| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :--- | --- | :---: | :--: | :--- |
| plate | 支付车牌 | string | Y | 粤TXXXXX |
| plate_color | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, <br>4.黑色, 5.绿色, -1 未知 | string | Y | 1 |
| plate_type | 车牌类型: 0-中国大陆车牌, 1-境外车牌(含港澳车牌), 2-两轮车(电动车/摩托车等) | string | N | 1 |
| card_id | 停车卡ID, 无车牌可传递 | string | N | ABDDS |
| gate_id | 缴费通道ID | string | N | 001 |
| parking_serial | 一次停车唯一ID | string | Y | XXXXX-ID |
| park_name | 停车场名称 | string | Y | XXXX-停车场 |
| enter_time | 入场时间, 单位毫秒 | string | Y | 1552976318722 |
| parking_time | 停车时长, 单位秒 | string | Y | 3600 |
| free_value | 车场优惠金额, 单位分 | int | Y | 100 |
| receipt | 微信点金计划页面显示按钮为`我要开票`, 1 显示, 其他无效 | string | N | 1 |


### I.II 固定车续费
```
trade_scene=RECHARGE 场景填写到extra字段
```
| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :--- | --- | :---: | :--: | :--- |
| plate | 支付车牌 | string | Y | 粤TXXXXX |
| plate_color | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, <br>4.黑色, 5.绿色, -1 未知 | string | Y | 1 |
| plate_type | 车牌类型: 0-中国大陆车牌, 1-境外车牌(含港澳车牌), 2-两轮车(电动车/摩托车等) | string | N | 1 |
| type | 固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡 | string | Y | 1 |
| charge_type | 固定车类型ID | string | Y | MON_001 |
| quantity | 续费数量:<br/>储值-续费金额, 单位分<br/>储次-续费次数<br/>储天-续费天数<br/>月卡-续费月数<br/>年卡-续费年数<br/>半年卡-续费半年数<br/>季度卡-续费季度数 | string | Y | 1 |
|renewal_start_time|时间类续费开始时间, 格式: yyyyMMdd| string | Y | 20230801 |
|renewal_end_time|时间类续费结束时间, 格式: yyyyMMdd| string | Y | 20230831 |
| receipt | 微信点金计划页面显示按钮为`我要开票`, 1 显示, 其他无效 | string | N | 1 |

### I.III 充电业务
```
trade_scene=ENERGY 场景填写到extra字段
```

| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :--- | --- | :---: | :--: | :--- |
| plate | 车牌 | string | Y | 川A660B1 |
| plate_color | 车牌颜色 | int | Y | 1 |
| device_no | 本地充电桩设备编号 | string | Y | |
| port_no | 本地充电桩设备枪号 | string | Y | |
| vin | 车辆标识 | string | N | |
| energy_code | 充电类型；CN_AC：慢充；CN_DC：快充 | string | Y | CN_AC |

### I.IV 生活缴费
```
trade_scene=LIVING 场景填写到extra字段
```

| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :--- | --- | :---: | :--: | :--- |
| room_no | 房号 | string | Y | 12栋-1单元-1301 |

## II. 坐标类型CoordType

| 值    | 描述       |
| ----- | ---------- |
| WGS84 | 地球坐标系 |
| BD09  | 百度坐标系 |
| GCJ02 | 火星坐标系 |


## III. 支付渠道定义

| 值    | 描述       |
| ----- | ---------- |
| 100002 | 支付宝APP支付 |
| 200001  | 微信APP支付 |
| 200201  | 微信JSAPI支付 |



## IV. 固定车类型

| 值    | 描述       |
| ----- | ---------- |
| 1 | 月卡 |
| 2  | 储值卡 |
| 3  | 储次 |
| 4  | 储天 |
| 5  | 年卡 |
| 6  | 季度卡 |
| 7  | 半年卡 |

## 颜色定义

<a id="color_type">`color_type`</a>

| 值  | 描述 |
|----|----|
| -1 | 未知 |
| 1  | 蓝色 |
| 2  | 黄色 |
| 3  | 白色 |
| 4  | 黑色 |
| 5  | 绿色 |
| 6  | 红色 |
| 7  | 银色 |
| 8  | 灰色 |
| 9  | 橙色 |

## 车类

<a id="car_type">`car_type`</a>

| 值  | 描述      |
|----|---------|
| -1 | 未知      |
| 1  | 临停车辆    |
| 2  | 包月车辆    |
| 3  | 贵宾/免费车辆 |
| 4  | 储值车辆    |


## 车型

<a id="vehicle_type">`vehicle_type`</a>

| 值  | 描述    |
|----|-------|
| -1 | 未知    |
| 1  | 小型车辆  |
| 2  | 大型车辆  |
| 3  | 超大型车辆 |
| 4  | 中型车辆  |
| 5  | 电动车 |