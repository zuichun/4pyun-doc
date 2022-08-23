# 出口无感扣款

### 请求地址

    https://api.4pyun.com/gate/1.0/parking/internal/prepay

### 调用方式

    HTTP FORM 表单提交

### 特殊说明:

    1:`auth_code`传了走被扫流程，如果不传该值会走无感流程特别注意无感扣款和被扫是同一个接口如果提示付款码无效，请重新扫码等提示多半是走传了`auth_code`走了被扫流程
    2:`pay_partner`本地扣费订单号, 同一个停车场唯一即使第一次失败了第二次也不能相同
    3:无感返回结果1001已扣款成功,1000代表收款请求已受理 1000的情况一般是无感平台不支持同步返回扣款结果
    4:https://api.4pyun.com/gate/1.0/parking/internal/payment 查询扣费订单，该接口是特殊场景发起支付之后如果想主动获取支付结果，不管这个接口返回状态是成功还是失败，最终还是要受理支付异步请求回调
    5:如果支付成功`pay_serial`一定会有,本地可以不处理这些值，如果想直接通过1001处理支付结果放行不等异步通知结果可以直接存储`pay_serial`,`pay_origin`,`pay_origin_desc`还是建议等异步通知逻辑简单
    6:app_id和api_type一般用不上停车场对接方式忽视掉，如果明确线下沟通了需要用到和研发同事沟通提供
    7:pay_partner`本地扣费订单号和异步通知`parking_order`是同一个值意义等价
    8:`free_value`,`total_value`,`pay_value`这些值传正确，传对了平台订单才能体现当前订单有优惠total_value=free_value+pay_value
    9:必须先推送入场才能发起无感扣款，否则一定扣款失败，配合无感状态下发接口使用，不要每个车到出口都发起扣款

### 请求参数

| 字段名称       | 字段说明                                             | 类型   | 必填 | 示例                                 |
| -------------- | ---------------------------------------------------- | ------ | :--: | ------------------------------------ |
| app_id         | 平台分配的接入应用ID, 在停车场内部使用接口时可不传递 | string |  N   | op1234567723122                      |
| api_type       | 接口类型配合app_id使用                               | string |  N   | api-debug                            |
| plate          | 车牌号码                                             | string |  Y   | 京A00001                             |
| enter_time     | 入场时间, 单位ms                                     | long   |  Y   | 1625154147313                        |
| total_value    | 总停车总费用, 单位分                                 | int    |  Y   | 1000                                 |
| pay_value      | 扣费金额, 单位分                                     | int    |  Y   | 1000                                 |
| free_value     | 优惠金额 没有优惠传0                                 | int    |  Y   | 0                                    |
| park_uuid      | 停车场UUID                                           | string |  Y   | 49f0cc52-e8c7-41e3-b54d-af666b8cc11a |
| parking_serial | 停车场端的停车流水, 一般为入场记录ID                 | string |  Y   | 111112                               |
| pay_partner | 本地扣费订单号, 同一个停车场唯一              | string |  Y   | 111112                               |
| parking_time   | 总停车时长, 单位秒                                   | int    |  Y   | 2000                                 |
| gate_id        | 出口扣款通道ID                                       | string |  N   | 1                                    |
| gate_name      | 出口扣款通道名称                                     | string |  N   | 通道1号                              |
| sign           | 请求数据签名                                         | string |  Y   | 37693CFC748049E45D87B8C7D8B9AACD     |

### 请求示例

```
public void testPrepay(){
        TreeMap<String, String> map = new TreeMap<>();
        // 车牌号码
        map.put("plate", "京A00001");
        // 扫码得到的值
        // map.put("auth_code", "135790356217418970");
        // 入场时间, 单位ms
        map.put("enter_time", (System.currentTimeMillis() - 1000 * 60 * 20) + "");
        // 优惠金额 没有优惠传0
        map.put("free_value", "0");
        // 停车场UUID
        map.put("park_uuid", "49f0cc52-e8c7-41e3-b54d-af666b8cc11a");
        // 停车场端的停车流水, 一般为入场记录ID
        map.put("parking_serial", "111112");
        // 总停车时长, 单位秒
        map.put("parking_time", "2000");
        // 本地扣费订单号, 同一个停车场唯一
        map.put("pay_partner", "11111114");
        // 扣费金额, 单位分
        map.put("pay_value", "10");
        // 总停车总费用, 单位分
        map.put("total_value", "10");
        // 通道
        map.put("gate_id", "通道ID");
        // 通道名称
        map.put("gate_name", "此路不通请绕行");
        StringBuilder builder = new StringBuilder();
        for (String key : map.keySet()) {
            builder.append(key + "=" + map.get(key) + "&");
        }
        String appSecret = "123";
        String encriptStr = builder.toString() + "app_secret=" + appSecret;
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        String keyStr = builder.toString() + "sign=" + sign;
        System.out.println(keyStr);
        try {
            Form form = Form.form();
            for (String key : map.keySet()) {
                form.add(key, map.get(key));
            }
            form.add("sign", sign);
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/internal/prepay")
                    .bodyForm(form.build(), Charset.forName("utf-8"))
                    .execute();
            HttpResponse response1 = response.returnResponse();
            System.out.println(response1.getStatusLine());
            String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
            System.out.println(text);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

### 请求返回结果参数说明

| 字段名称        | 字段说明       |  类型  | 必填 | 备注                                                         |
| :-------------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code            | 请求状态码     | string |  Y   | 1000:收款请求已受理<br>1001:已扣款成功<br>400:参数错误<br>500:处理失败, 提示message的信息<br>503:服务暂不可用 |
| message         | 返回描述       | string |  Y   | 返回描述                                                     |
| hint            | 返回错误说明   | string |  N   | 返回具体错描述指导                                           |
| seqno           | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值                               |
| pay_id          | 支付结果ID     | string |  N   | 123456789                                                    |
| pay_serial      | 平台支付流水   | string |  N   | 123456789                                                    |
| pay_origin      | 到账方式       | string |  N   | 1                                                            |
| pay_origin_desc | 到账方式描述   | string |  N   | P云                                                          |


### 请求返回结果示例:


```
正常返回
{
    "code": "1001",
    "seqno": "4b1889bde31f0561",
    "data_node": "CN-South/HS3-3",
    "time_cost": 8
}
```

```
参数错误 参数少传了
{
    "code": "1000",
    "message": "受理成功",
    "seqno": "4b1889bde31f0561",
    "data_node": "DEV/DEV-1",
    "path": "POST /gate/1.0/parking/internal/prepay"
}
```

```
{
    "code": "400",
    "message": "请求参数错误",
    "hint": "检查请求参数`enter_time`!",
    "seqno": "aef8bf7fe6664254",
    "data_node": "DEV/DEV-1",
    "path": "POST /gate/1.0/parking/internal/prepay"
}
```

```
// 服务器内部错误
{
    "code": "500",
    "message": "未匹配到停车记录",
    "seqno": "1229ecc07700c7c1",
    "data_node": "CN-South/HS3-1",
    "path": "POST /gate/1.0/parking/internal/prepay"
}
```

```
// 服务器内部错误
{
    "code": "503",
    "message": "服务暂不可用",
    "hint": "NO HEALTH SERVICE(ParkingSyncer)!",
    "seqno": "7fb563f1fc2ca1f",
    "data_node": "DEV/DEV-1",
    "path": "POST /gate/1.0/parking/internal/prepay"
}
```
