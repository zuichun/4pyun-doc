# 营销活动用户校验

## <a name="callback"><font color="#33333">奖励优惠券回调</font></a>

* 描述

  通过URL传参，及form表单传参的⽅式回调接⼝实现⽅。

### <a name="callback_precheck"><font color="#33333">奖励优惠券领取校验</font></a>

* 描述

  合作⽅实现该接⼝，⽤于查询⽤户是否符合领券要求。

* 请求⽅式

  ``` POST ```

* 请求参数

| 字段      | 类型   | 必须 | 说明         |
| --------- | ------ | ---- | ------------ |
| user_code | string | Y    | ⽤户唯⼀标识 |
| mobile    | string | N    | ⼿机号       |

* 响应参数

| 字段    | 类型   | 必须 | 说明             |
| ------- | ------ | ---- | ---------------- |
| code    | string | Y    | 业务处理状态码   |
| message | string | N    | 业务处理状态说明 |
| hint    | string | N    | 提示说明         |
