# 营销活动领取回调

* 描述

  合作⽅实现该接⼝，⽤于通知⽤户领券结果。

* 请求方式

  ``` POST ```

* 请求参数

| 字段        | 类型      | 必须 | 说明                                      |
| ----------- | --------- | ---- | ----------------------------------------- |
| user_code   | string    | Y    | ⽤户唯⼀标识                              |
| mobile      | string    | N    | ⼿机号                                    |
| status      | short     | Y    | 状态，0未发放，1发放中，2已发放，-1已失效 |
| bonus_id    | string    | Y    | 奖励优惠券ID                              |
| value       | int32     | Y    | 奖励⾯额，单位分                          |
| effect_time | timestamp | Y    | ⽣效时间                                  |
| expire_time | timestamp | Y    | 失效时间                                  |

* 响应参数

| 字段    | 类型   | 必须 | 说明             |
| ------- | ------ | ---- | ---------------- |
| code    | string | Y    | 业务处理状态码   |
| message | string | N    | 业务处理状态说明 |
| hint    | string | N    | 提示说明         |
