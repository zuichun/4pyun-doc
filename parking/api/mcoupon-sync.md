# 商户优惠券状态同步

### 请求地址

	https://api.4pyun.com/gate/1.0/parking/internal/mcoupon/grant/sync

### 调用方式

	HTTP FORM 表单提交

### 特殊说明


### 请求参数

| 字段名称     | 字段说明                             | 类型    | 必填 | 示例                             |
| ------------ | ------------------------------------ | ------- | ---- | -------------------------------- |
| app_id       | 应用ID                               | string  | Y    | opxxxxx                          |
| sign         | 请求数据签名                         | string  | Y    | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| park_uuid    | 停车场UUID                           | string  | Y    |                                  |
| grant_serial | 平台下发的派发流水                   | string  | Y    |                                  |
| status       | 更新派发状态固定值-2                 | string  | Y    |                                  |
| status_desc  | 更新派发状态说明                     | string  | Y    | 已过期                           |
| status_time  | 状态变化时间, 默认取当前收到请求时间 | utc时间 | N    | 2022-02-28T03:30:28.000Z         |


### 请求返回结果参数说明
| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                                         |
| :------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001：成功 其它失败 |
| message  | 返回描述       | string |  Y   | 返回描述                                                     |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |
