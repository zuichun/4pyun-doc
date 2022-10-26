# 推送同步固定车辆信息

###  请求地址

	https://api.4pyun.com/gate/1.0/parking/internal/vip

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

| 字段名称      | 字段说明                                                     |  类型  | 必填 | 示例  |
| :------------ | :----------------------------------------------------------- | :----: | :--: | :---- |
| plate         | 车牌号码                                                     | string |  Y   |       |
| plate_color   | 车辆颜色: <br>1.蓝色<br/>2.黄色<br/>3.白色<br/>4.黑色<br/>5.绿色<br/>-1 未知 |  int   |  Y   |       |
| card_no       | 逻辑卡号                                                     | string |  N   |       |
| card_id       | 物理卡号                                                     | string |  N   |       |
| charge_type   | 固定车收费类型, 一般是收费规则ID<br>和`service.parking.charge_type.list`返回的`id`字段含义相同 | string |  Y   | MON_A |
| charge_desc   | 当前固定车收费标准描述                                       | string |  Y   |       |
| status        | 固定车状态1:正常 0:停用 -1:删除                              |  int   |  Y   |       |
| create_time   | 发卡时间, 时间戳单位ms                                       | string |  Y   |       |
| expire_time   | 过期时间, 时间戳单位ms                                       | string |  Y   |       |
| type          | 固定车类型:<br/>1 月卡<br/>2 储值<br/>3 储次<br/>4 储天<br/>5 年卡<br/>6 季度卡<br/>7 半年卡<br/>0 免费停车(白名单) |  int   |  Y   |       |
| balance       | 当前余额<br/>储值: 为余额, 单位分;<br/>储次:为剩余次数;<br/>储天:为剩余天数;<br/>月卡/年卡/半年卡/季度卡:为剩余天数.<br>时间卡若当天过期则=0 |  int   |  Y   |       |
| realname      | 车主姓名                                                     | string |  Y   |       |
| mobile        | 车主联系电话                                                 | string |  N   |       |
| id_card       | 车主身份证号码                                               | string |  N   |       |
| address       | 车主单位/住宅                                                | string |  N   |       |
| park_space_no | 车位号                                                       | string |  N   |       |
| operator      | 操作员                                                       | string |  N   |       |

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
    public void testVipUpload(){
        TreeMap<String, Object> map = new TreeMap<>();
        // 停车场ID
        map.put("park_uuid", "xxxxxx");
        // 全量数据标记: 1是, 0否(默认) 不是全量可以不传
        map.put("complete","0");
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
        // 固定车收费描述
        map1.put("charge_desc","月卡A");
        // 固定车状态: 1.正常, 0.停用, -1.删除
        map1.put("status","1");
        // 发卡时间, 时间戳单位ms
        map1.put("create_time",DateUtils.parse("2021-05-30 23:59:59","yyyy-MM-dd HH:mm:ss").getTime()+"");
        // 过期时间, 时间戳单位ms
        map1.put("expire_time",DateUtils.parse("2022-05-30 23:59:59","yyyy-MM-dd HH:mm:ss").getTime()+"");
        // 固定车类型: 1.包月, 2.储值, 5.年卡, 6.季度卡, 7.半年卡, 0.免费卡
        map1.put("type","1");
        // 时间类(月卡/年卡等) - 剩余天数, 若当天过期则=0, 昨天过期=-1, 以此类推。
        // 储值卡 - 剩余金额, 单位分。
        // 面额类(次卡等) - 剩余次数
        map1.put("balance","365");
        // 车主姓名
        map1.put("realname","张三");
        // 车主联系电话
        map1.put("mobile","88888888");
        // 车主身份证号码
        map1.put("id_card","4310281999999999999");
        // 车主单位/住宅
        map1.put("address","A栋1001号");
        // 车位号
        map1.put("park_space_no","001");
        // 操作员
        map1.put("operator","张三");
        // VIP列表
        map.put("row", Lists.newArrayList(map1));
        String appSecret = "123werer";
        String content = JSON.toJSONString(map);
        String encriptStr = content + "&app_secret=" + appSecret;
        System.out.println(encriptStr);
        String sign = MD5.encryptHEX(encriptStr);
        System.out.println("sign:"+sign);
        System.out.println(encriptStr);
        try {
            Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/internal/vip")
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
