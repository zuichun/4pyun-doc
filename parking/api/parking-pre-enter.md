# 推送车辆预入场

###  请求地址

	https://api.4pyun.com/gate/1.0/parking/internal/enter/before

###  调用方式

	HTTP POST  FORM 表单提交

###  特殊说明

车辆到入口上报预入场 返回是否允许入场 如果允许入场可以返回现场计费规则ID 对车辆进行指定特定计费规则


###  请求参数

| 字段名称      | 字段说明                                                     |  类型  | 必填 | 示例                             |
| :------------ | :----------------------------------------------------------- | :----: | :--: | :------------------------------- |
| app_id        | 平台分配的接入应用ID, 车场本地发起请求可不传递         | string |  N   | op1234567723122                  |
| api_type      | 接口类型     | string |  N   | XXX-SERVICE                      |
| sign          | 请求数据签名                                                 | string |  Y   | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| park_uuid     | 平台分配的停车场UUID                                         | string |  Y   | PARKUUID-XXXX-XXX-XXX            |
| plate         | 车牌号码                                                     | string |  Y   | 粤B12345                         |
| plate_color   | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知    | short |  Y   | 1                                |
| gate_time | 通道时间, 单位ms                                             | string |  Y   | 1552976318722                    |
| gate_id | 通道ID                                                   | string |  Y   | 001                              |
| gate_name    | 通道名称                                                     | string |  Y   | 东门入口                         |
| capture_image  | 当前抓拍图片  | string |  N   | 上传当前抓拍图片 |
| vehicle_type  | 车型: 1.小车, 2.大车, -1.未知                                | short |  N   | 1                                |

###  请求返回结果参数说明

| 字段名称       | 字段说明                   |  类型  | 必填 | 备注                                                         |
| :------------- | :------------------------- | :----: | :--: | :----------------------------------------------------------- |
| code           | 请求状态码                 | string |  Y   | 1001:查询成功<br>1002:查询无结果<br>其它状态:查看message内容 |
| message        | 返回描述                   | string |  Y   | 返回描述                                                     |
| hint           | 返回错误说明               | string |  N   | 返回具体错描述指导                                           |
| seqno          | 服务器日志标示             | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| status         | 车辆状态: 1允许入场, 0本地处理,  -1不允许入场 | int32 |  Y   | 1   |
| status_desc   | 车辆状态说明, 返回后显示到LED屏幕 | string |  N   | 黑名单车辆禁止通行  |
| charge_rule | 计费规则ID status=1才生效  | string |  N   | rule-xxxxx                                                   |
