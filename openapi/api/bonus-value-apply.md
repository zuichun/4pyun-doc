
### <a name="callback_use"><font color="#33333">奖励优惠券使⽤通知</font></a>

* 描述

  合作⽅实现该接⼝，接受⽤户⽤券通知。
  bonus_id和提交奖励事件参与申请的应答消息bonus节点的id属性具有关联性，
  接⼝实现⽅匹配每条奖励记录进⾏核销结果处理

* 请求方式

  ``` POST ```

* 请求参数

| 字段       | 类型      | 必须 | 说明             |
| ---------- | --------- | ---- | ---------------- |
| user_code  | string    | Y    | ⽤户唯⼀标识     |
| mobile     | string    | N    | ⼿机号           |
| status     | short     | Y    | 状态，1已核销    |
| bonus_id   | string    | Y    | 奖励优惠券ID     |
| value      | int32     | Y    | 核销⾦额，单位分 |
| apply_time | timestamp | Y    | 核销时间         |

* 响应参数

| 字段    | 类型   | 必须 | 说明             |
| ------- | ------ | ---- | ---------------- |
| code    | string | Y    | 业务处理状态码, 1001-处理成功, 1500-处理异常   |
| message | string | N    | 业务处理状态说明 |
| hint    | string | N    | 提示说明         |
