# 营销活动库存查询

* 描述

  P云为接⼝提供⽅，⽤于查询奖励事件列表。

* 请求方式

  ``` GET ```

* 请求地址

    ``` /gate/1.0/bonus/event/stocks ```

* 请求参数

| 字段     | 类型   | 必须 | 说明               |
| -------- | ------ | ---- | ------------------ |
| app_id   | string | Y    | 应⽤ID,由P云分配   |
| event_id | string | Y    | 活动编号,由P云分配 |

* 响应参数

| 字段                | 类型      | 必须 | 说明                       |
| ------------------- | --------- | ---- | -------------------------- |
| name                | string    | N    | 奖励事件名称               |
| value               | int32     | Y    | 面额:分                    |
| apply_desc          | string    | N    | 描述                       |
| quantity            | int32     | Y    | 单次派发张数               |
| effect_time         | timestamp | Y    | 生效时间                   |
| expire_time         | timestamp | Y    | 失效时间                   |
| stock               | int32     | Y    | 总数量                     |
| grant_quantity      | int32     | Y    | 已发放数量                 |
| daily_limit_value   | int32     | N    | 单天最高消耗金额，单位：分 |
| apply_minimum_value | int32     | N    | 使用券金额门槛，单位：分   |
| user_limit_quantity | int32     | N    | 单个用户可领个数           |
