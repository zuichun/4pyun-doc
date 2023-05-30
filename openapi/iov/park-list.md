# 停车场列表查询

### 概述

此接口用于查询 **已授权 ** 的停车场列表。用于以下场景：

- 通过经纬度半径查询周边停车场（不限制 `city`）；
- 通过 `city` 查询该城市的停车场（不限制经纬度半径）。

### 接口定义

接口名称：`gate/1.0/parking/park/list`

请求方式：`GET`

使用方法：P云实现，由需求方调用。

### 请求

#### 参数

| 参数       | 类型   | 必须 | 描述                    | 示例值          |
| ---------- | ------ | ---- | ----------------------- | --------------- |
| app_id     | string | Y    | 平台分配的接入应用ID    | op1234567723122 |
| sign       | string | Y    | 签名                    |                 |
| latitude   | double | N    | 维度                    | 川A660PP        |
| longitude  | double | N    | 经度                    | 1               |
| radius     | int    | N    | 半径，米                | 1000            |
| coord_type | string | N    | 坐标系类型，默认：WGS84 | WGS84           |
| city       | string | N    | 城市                    | 深圳市          |
| ...        |        |      |                         |                 |

#### 示例

```json
{
    "coord_type": "BD09",
    "latitude": "22.535853",
    "page_index": "1",
    "sign": "10DFF44B4E123BE7938810BC96172A58",
    "radius": "10000",
    "app_id": "op00961963581daa7",
    "longitude": "113.958233",
    "page_size": "10"
}
```

### 响应

#### 参数

| 参数               | 类型     | 必须 | 描述                                                         | 示例值                                    |
| ------------------ | -------- | ---- | ------------------------------------------------------------ | ----------------------------------------- |
| park_uuid          | string   | Y    | P云车场标识                                                  |                                           |
| park_name          | string   | Y    | P云车场名称                                                  | P云支付体验-停车场                        |
| address            | string   | Y    | 详细地址                                                     | 广东省深圳市南山区深圳湾科技生态园10栋    |
| latitude           | double   | Y    | 纬度                                                         | 22.535853                                 |
| longitude          | double   | Y    | 经度                                                         | 113.958233                                |
| coord_type         | string   | Y    | <a href="https://doc.4pyun.com/appendix#coord_type">坐标系类型</a>，默认：WGS84 | WGS84                                     |
| province           | string   | Y    | 省                                                           | 广东省                                    |
| city               | string   | Y    | 市                                                           | 深圳市                                    |
| district           | string   | Y    | 区                                                           | 南山区                                    |
| area_code          | string   | Y    | 区域编码                                                     | 440305                                    |
| distance           | int      | Y    | 距离，米                                                     | 10                                        |
| total_space        | int      | Y    | 总车位数                                                     | 100                                       |
| remain_space       | int      | Y    | 剩余车位数                                                   | 30                                        |
| charge_desc        | string   | Y    | 收费规则描述                                                 | 首小时4元, 之后1元/小时, 全天最高收费10元 |
| charge_simple_desc | string   | Y    | 收费规则简单描述                                             | 首小时4元,之后1元/小时                    |
| charge_depict      | string   | Y    | 收费规则JSON数据                                             |                                           |
| enter_free_time    | int      | Y    | 入场免费时间，分钟                                           | 30                                        |
| leave_buffer_time  | int      | Y    | 出场预留时间，分钟                                           | 10                                        |
| image_url          | string[] | Y    | 车场图片集合                                                 |                                           |
| tag                | string[] | Y    | <a href="https://doc.4pyun.com/appendix#park_tag">车场标签集合</a> |                                           |
| ...                |          |      |                                                              |                                           |

#### 示例

```json
{
    "code": "1001",
    "seqno": "50549897049829708931322006039855",
    "data_node": "CN-South/DEV-1",
    "time_cost": 443,
    "payload": {
        "paging_header": {
            "total_count": 7246,
            "page_index": 1,
            "page_size": 10,
            "page_count": 725,
            "paging_timestamp": "1970-01-01T00:00:00Z"
        },
        "row": [
            {
                "park_uuid": "49f0cc52-e8c7-41e3-b54d-af666b8cc11a",
                "merchant": "",
                "park_name": "P云支付体验-停车场",
                "address": "广东省深圳市南山区沙河西路辅路与高新南环路交汇处西南角深圳湾科技生态园",
                "latitude": 22.535598366761718,
                "longitude": 113.95907590895133,
                "province": "广东省",
                "city": "深圳市",
                "district": "南山区",
                "area_code": "440305",
                "total_space": 300,
                "remain_space": 2,
                "realtime": 0,
                "first_charge_time": 0,
                "first_charge_value": 0,
                "charge_desc": "首小时4元,之后1元/小时，过夜收费40元",
                "charge_simple_desc": "首小时4元,之后1元/小时",
                "charge_extra_desc": "",
                "enter_free_time": 0,
                "leave_buffer_time": 1,
                "image_url": [
                    
                ],
                "distance": 91,
                "tag": [
                    "测试"
                ],
                "coord_type": "BD09",
                "accept_recharge": 0,
                "corp_status": 0
            },
            {
                "park_uuid": "f6a091a5-6997-48b6-b2e8-79eb347e9c21",
                "merchant": "",
                "park_name": "豪威科技大厦-停车场",
                "address": "广东省深圳市南山区高新南七道",
                "latitude": 22.54188232918348,
                "longitude": 113.97008957875683,
                "province": "广东省",
                "city": "深圳市",
                "district": "南山区",
                "area_code": "440305",
                "total_space": 250,
                "remain_space": 1,
                "realtime": 0,
                "first_charge_time": 0,
                "first_charge_value": 0,
                "charge_desc": "首小时10元,之后每小时3元,全天最高35元。不足1小时按1小时计",
                "charge_simple_desc": "首小时10元,之后3元/小时",
                "charge_extra_desc": "",
                "enter_free_time": 15,
                "leave_buffer_time": 15,
                "image_url": [
                    
                ],
                "distance": 350,
                "tag": [
                    "商业办公楼"
                ],
                "coord_type": "BD09",
                "accept_recharge": 0,
                "corp_status": 0
            }
        ]
    }
}
```

#### 响应码

| 值   | 描述                  |
| ---- | --------------------- |
| 401  | 应用不存在、签名错误  |
| 403  | 接口尚未授权          |
| 1001 | 操作成功              |
| 1002 | 未查询到数据          |
| 1500 | 内部错误，参考message |

### 数据权限等级

<table>
  <tr>
    <th>参数</th>
    <th>描述</th>
    <th>权限</th>
  </tr>
  <tr>
    <td>park_uuid</td>
    <td>车场标识</td>
  	<td rowspan="13">基础数据（1级）</td>
  </tr>
  <tr>
    <td>park_name</td>
    <td>车场名称</td>
  </tr>
  <tr>
    <td>address</td>
    <td>详细地址</td>
  </tr>
  <tr>
    <td>latitude</td>
    <td>纬度</td>
  </tr>
  <tr>
    <td>longitude</td>
    <td>经度</td>
  </tr>
  <tr>
    <td>coord_type</td>
    <td><a href="https://doc.4pyun.com/appendix#coord_type">坐标系类型</a></td>
  </tr>
  <tr>
    <td>province</td>
    <td>省</td>
  </tr>
  <tr>
    <td>city</td>
    <td>市</td>
  </tr>
  <tr>
    <td>district</td>
    <td>区</td>
  </tr>
  <tr>
    <td>area_code</td>
    <td>区域编码</td>
  </tr>
  <tr>
    <td>distance</td>
    <td>距离</td>
  </tr>
  <tr>
    <td>image_url</td>
    <td>车场图片</td>
  </tr>
  <tr>
    <td>tag</td>
    <td><a href="https://doc.4pyun.com/appendix#park_tag">车场标签</a></td>
  </tr>
  <tr>
    <td>total_space</td>
    <td>总车位数</td>
    <td rowspan="2">车位数据（2级）</td>
  </tr>
  <tr>
    <td>remain_space</td>
    <td>剩余车位数</td>
  </tr>
  <tr>
    <td>charge_desc</td>
    <td>计费规则描述</td>
    <td rowspan="4">计费规则数据（4级）</td>
  </tr>
  <tr>
    <td>charge_simple_desc</td>
    <td>计费规则简单描述</td>
  </tr>
  <tr>
    <td>enter_free_time</td>
    <td>入场免费时间</td>
  </tr>
  <tr>
    <td>leave_buffer_time</td>
    <td>离场预留时间</td>
  </tr>
  <tr>
    <td>charge_depict</td>
    <td>计费规则JSON数据</td>
    <td rowspan="1">计费规则JSON数据（8级）</td>
  </tr>
</table>
