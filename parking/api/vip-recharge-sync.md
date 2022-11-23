# 推送同步固定车续费记录

###  请求地址

	https://api.4pyun.com/gate/1.0/parking/internal/vip/recharge

###  调用方式

	HTTP POST JSON

###  特殊说明

```
1：供车场批量同步本地固定车信息到平台, 第一次本地需将本地存量的记录分批次同步发送, 存量数据同步完成后后续的增量数据实时同步即可。<br/><br/>仅支持POST JSON
2：请求签名得到的值放header请求头里面的 Authorization 字段对应的值 详细参考下面测试用例写法
```


###  请求参数

| 字段名称  | 字段说明                                                     |  类型  | 必填 | 示例                  |
| :-------- | :----------------------------------------------------------- | :----: | :--: | :-------------------- |
| park_uuid | 平台分配的停车场UUID                                         | string |  Y   | PARKUUID-XXXX-XXX-XXX |
| complete  | 全量数据标记1:删除平台已有全部数据 0:不删除历史数据正常新增数据（传1的时候慎重一般适用于第一次传历史全量数据） |  int   |  N   | 0                     |
| row       | 同步记录合集（参考ParkingVIP）数组类型[{},{},{}]             | string |  Y   |                       |



###  请求参数row具体项ParkingVIP

| 字段名称    | 字段说明                                                     |  类型  | 必填 | 示例  |
| :---------- | :----------------------------------------------------------- | :----: | :--: | :---- |
| plate       | 车牌号码                                                     | string |  Y   |       |
| plate_color | 车辆颜色: <br>1.蓝色<br/>2.黄色<br/>3.白色<br/>4.黑色<br/>5.绿色<br/>-1 未知 |  int   |  Y   |       |
| card_no     | 逻辑卡号                                                     | string |  N   |       |
| card_id     | 物理卡号                                                     | string |  N   |       |
| charge_type | 固定车收费类型, 一般是收费规则ID<br>和`service.parking.charge_type.list`返回的`id`字段含义相同 | string |  Y   | MON_A |
| vip_type    | 固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡<br/>0 免费停车(白名单) | string |  Y   |       |
| quantity    | 续费单位数量, 例如一个月、一季度、一年等                     |  int   |  Y   |       |
| pay_partner | 车场订单号                                                   |  int   |  Y   |       |
| pay_type    | 支付类型<br/>1: 现金支付;<br/>2:在线支付;                    |  int   |  Y   |       |
| value       | 支付面额: 储值卡为金额, 次卡为次数, 时间卡为天数             | string |  N   |       |
| pay_time    | 支付时间, 时间戳单位ms                                       | string |  N   |       |
| renewal_start_time    | 续费开始时间, UTC时间 示例时间代表2022-10-01 00:00:00 | string |  N   | 2022-09-30T16:00:00Z      |
| renewal_end_time    | 续费结束时间, UTC时间 示例时间代表2022-10-31 23:59:59  | string |  N   |  2022-10-31T15:59:59Z     |
| pay_value   | 支付金额, 单位分                                             |  int   |  N   |       |
| operator    | 操作员                                                       | string |  N   |       |

###  

###  请求返回结果参数说明

| 字段名称 | 字段说明       |  类型  | 必填 | 备注                                      |
| :------- | :------------- | :----: | :--: | :---------------------------------------- |
| code     | 请求状态码     | string |  Y   | 1001:查询成功<br>其它状态:查看message内容 |
| message  | 返回描述       | string |  Y   | 返回描述                                  |
| hint     | 返回错误说明   | string |  N   | 返回具体错描述指导                        |
| seqno    | 服务器日志标示 | string |  Y   | 查日志用到查问题尽量提供这个值            |

示例代码

```
    @Test
    public void testVipRechargeUpload(){
        TreeMap<String, Object> map = new TreeMap<>();
        // 停车场ID
        map.put("park_uuid", "49f0cc52-e8c7-41e3-b54d-af666b8cc11a");
        Map<String,String> map1 = new HashMap<>();
        // 车牌
        map1.put("plate","粤X33333");
        // 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知
        map1.put("plate_color","1");
        // 逻辑卡号
        map1.put("card_no","ID000001");
        // 物理卡号
        map1.put("card_id","ID000001");
        // 固定车收费类型, 一般是收费规则ID
        map1.put("charge_type","101");
        // 固定车类型: 1.包月, 2.储值, 5.年卡, 6.季度卡, 7.半年卡, 0.免费卡
        map1.put("vip_type","1");
        // 续费单位数量, 例如一个月、一季度、一年等
        map1.put("quantity","1");
        // 车场订单号停车场唯一
        map1.put("pay_partner","pay_partner_1234567890");
        // 1 现金支付
        map1.put("pay_type","1");
        // 支付面额: 储值卡为金额, 次卡为次数, 时间卡为天数
        map1.put("value","30");
        // 支付时间, 时间戳单位ms
        map1.put("pay_time",DateUtils.parse("2021-06-30 23:59:59","yyyy-MM-dd HH:mm:ss").getTime()+"");
        // 支付金额, 单位分
        map1.put("pay_value","10000");
        // 操作员
        map1.put("operator","张三");
        // VIP列表
        map.put("row", Lists.newArrayList(map1));
        String appSecret = "12323232323";
        String content = JSON.toJSONString(map);
        String encriptStr = content + "&app_secret=" + appSecret;
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        System.out.println("sign:"+sign);
        System.out.println(encriptStr);
        try {
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/internal/vip/recharge")
                    .addHeader("Authorization",sign)
                    .bodyString(content,ContentType.APPLICATION_JSON)
                    .execute();
            HttpResponse response1 = response.returnResponse();
            System.out.println(response1.getStatusLine());
            String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
            System.out.println(text);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
