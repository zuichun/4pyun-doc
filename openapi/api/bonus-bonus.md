# 5.2 查询营销活动查询奖励


### <a name="bonus_detail"><font color="#33333">查询奖励详情</font></a>

* 描述

  P云为接⼝提供⽅，⽤于查询奖励详情。

* 请求方式

  ``` GET```
* 请求地址

  ``` /gate/1.0/bonus/bonus ```

* 请求参数

| 字段   | 类型   | 必须 | 说明             |
| ------ | ------ | ---- | ---------------- |
| app_id | string | Y    | 应⽤ID,由P云分配 |
| id     | int64  | Y    | 奖励ID           |

* 响应参数

| 字段         | 类型          | 必须 | 说明                                 |
| ------------ | ------------- | ---- | ------------------------------------ |
| id           | int64         | Y    | 奖励ID                               |
| cover        | string        | N    | 奖励封⾯                             |
| value        | int32         | Y    | 奖励⾯额，单位分                     |
| quantity     | int32         | Y    | 奖励数量                             |
| identity     | string        | Y    | ⽤户ID,登录授权获取                  |
| status       | int32         | Y    | 奖励状态：0未发放, 1 发放中，2已领取 |
| api_bonus_id | string        | N    | 微信等第三方券编码                   |
| grant_time   | timestamp     | N    | 发放时间                             |
| apply_status | int32         | Y    | 核销状态：0未核销，1 已核销          |
| apply_time   | timestamp     | N    | 核销时间                             |
| effect_time  | timestamp     | N    | 生效时间                             |
| expire_time  | timestamp     | N    | 失效时间                             |
| issuer       | string        | N    | 发放人                               |
| apply_desc   | string        | N    | 奖励描述                             |
| remark       | string        | N    | 备注                                 |
| children     | array[object] | N    | 子奖励记录                           |

​	children

| 字段         | 类型      | 必须 | 说明                                 |
| ------------ | --------- | ---- | ------------------------------------ |
| id           | int64     | N    | 奖励ID                               |
| value        | int32     | N    | 奖励⾯额，单位分                     |
| status       | int32     | N    | 奖励状态：0未发放, 1 发放中，2已领取 |
| api_bonus_id | string    | N    | 微信等第三方券编码                   |
| grant_time   | timestamp | N    | 发放时间                             |
| apply_status | int32     | N    | 核销状态                             |
| apply_time   | timestamp | N    | 核销时间                             |
| effect_time  | timestamp | N    | 生效时间                             |
| expire_time  | timestamp | N    | 失效时间                             |
