# 推送同步黑名单车辆信息

###  请求地址

	https://api.4pyun.com/gate/1.0/parking/internal/blacklist

###  调用方式

	HTTP POST JSON

###  特殊说明

```
供车场批量同步本地固定车信息到平台, 第一次本地需将本地存量的记录分批次同步发送, 存量数据同步完成后后续的增量数据实时同步即可仅支持POST JSON
请求签名得到的值放header请求头里面的 Authorization 字段对应的值 详情参考一下固定车新增测试用例写法
```


###  请求参数

| 字段名称  | 字段说明                                                     |  类型  | 必填 | 示例                  |
| :-------- | :----------------------------------------------------------- | :----: | :--: | :-------------------- |
| park_uuid | 平台分配的停车场UUID                                         | string |  Y   | PARKUUID-XXXX-XXX-XXX |
| complete  | 全量数据标记1:删除平台已有全部数据 0:不删除历史数据正常新增数据（传1的时候慎重一般适用于第一次传历史全量数据） |  int   |  N   | 0                     |
| row       | 同步记录合集（参考ParkingBlacklist）数组类型[{},{},{}]       | string |  Y   |                       |



###  请求参数row具体项ParkingBlacklist

| 字段名称    | 字段说明                                                     |  类型  | 必填 | 示例 |
| :---------- | :----------------------------------------------------------- | :----: | :--: | :--- |
| plate       | 车牌号码                                                     | string |  Y   |      |
| plate_color | 车辆颜色: <br>1.蓝色<br/>2.黄色<br/>3.白色<br/>4.黑色<br/>5.绿色<br/>-1 未知 |  int   |  Y   |      |
| card_id     | 物理卡号                                                     | string |  N   |      |
| realname    | 车主姓名                                                     | string |  Y   |      |
| reason      | 原因                                                         | string |  N   |      |
| status      | 黑名单状态: 1.正常, 0.停用, -1.删除                          |  int   |  Y   |      |
| create_time | 创建时间, 时间戳单位ms                                       | string |  Y   |      |
| expire_time | 数据更新时间, 时间戳单位ms                                   | string |  Y   |      |
| start_time  | 拉黑开始时间, 时间戳单位ms                                   | string |  Y   |      |
| end_time    | 拉黑结束时间, 时间戳单位ms                                   | string |  Y   |      |
| operator    | 操作员                                                       | string |  N   |      |

###  

###  请求返回结果参数说明

| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                      |
| :------- | :------------- | :----: | :--: | :---------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:查询成功<br>其它状态:查看message内容 |
| message  | 返回描述       | string |  Y   | 返回描述                                  |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                        |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值            |

