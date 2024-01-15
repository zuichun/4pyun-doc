# 营销活动参与申请


### <a name="bonus_apply"><font color="#33333">提交奖励事件参与申请</font></a>

* 描述
  P云为接⼝提供⽅，⽤于发放奖励。

  会进行重复参与活动判断。

  当voucher为空，event_id+identity标识唯⼀记录。

  否则，event_id+voucher作为请求发起⽅的唯⼀标记。

* 请求方式

  ``` POST ```

* 请求地址

  ``` /gate/1.0/bonus/event/apply ```

* 请求参数

| 字段     | 类型   | 必须 | 说明                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| event_id | string | Y    | 活动编号,由P云分配                                           |
| identity | string | Y    | ⽤户ID,登录授权获取                                          |
| app_id   | string | Y    | 应⽤ID,由P云分配                                             |
| openid   | string | N    | 微信/⽀付宝OPENID                                            |
| fields   | object | N    | 申请表内容。当合作方实现了回调时，需填充key为user_code, value为合作方用户标识的键值对 |
| issuer   | string | N    | 发放⼈                                                       |
| voucher  | string | N    | 凭证,例如业务订单号                                          |
| mobile   | string | N    | ⼿机号应答参数                                               |

* 响应参数

| 字段  | 类型          | 必须 | 说明     |
| ----- | ------------- | ---- | -------- |
| bonus | array[object] | Y    | 活动奖励 |

​		bonus

| 字段     | 类型   | 必须 | 说明             |
| -------- | ------ | ---- | ---------------- |
| id       | int64  | Y    | 奖励ID           |
| cover    | string | N    | 奖励封⾯         |
| name     | string | N    | 奖励名称         |
| value    | int32  | Y    | 奖励⾯额，单位分 |
| quantity | int32  | N    | 奖励数量         |
| status   | int32  | Y    | 奖励状态,0未发放 |
| remark   | string | N    | 备注             |
| issuer   | string | N    | 发放⼈           |
| identity | string | N    | 领取⽤户ID       |
