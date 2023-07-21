# 推送车辆离场

### 请求地址
	https://api.4pyun.com/gate/1.0/parking/internal/leave

### 调用方式
	HTTP FORM 表单提交

### 特殊说明

<b>签名计算:
</b><br/>
如果上传文件`enter_image_file`时, `enter_image_file`文件流只参与计算得出`enter_image_hash`的值; `enter_image_hash`参与最终请求`sign`的计算 `enter_plate_image_file` 同理。
<br/><br/>
 <b>注意事项</b><br/>
 1:同一停车场`parking_serial`不能重复, 重复的流水将被忽略!<br>
 2:一般情况下不传车牌图片相关字段如`enter_plate_image`,`enter_plate_image_file`,`enter_plate_image_hash`,该字段是特殊城市平台既要车牌拍照图片，还需要上传单独的车牌识别图片即要把大的图片里面抠出来车牌部分图片<b>出场对应车牌图片部分同理</b>
 3:传文件有三种方式下面以入场图片为例子，后面对文件说明直接复用该地方说明<br>
 3.1:有外网可访问图片直接传`enter_image`<br>
 3.2:传文件字节流传了`enter_image_file`字段 `enter_image`会被忽视<br>
4:通行证信息`idcard_name`,`idcard_no`,`idcard_image`是部分景区强制要求上传需要用到的才传<br/>
 <b>5:默认城市平台上传车位数据是入一辆车车位减一，出一辆车车位加一，如果中途出现断传等现象会导致下次重新上传数据不准确，针对这种情况平台建议如果本地有条件把剩余车位`remain_parking_space`传上来这样就不需要平台人工干预车位数了</b><br>
 6:开发对接过程中签名错误等信息不会直接返回错误状态码，对接过程中就判断返回结果里面是否有hint值，如果有根据hint的返回查错误原因<br>
 正式上线之后只判断code是否为200，200代表成功
 <br/>
 7:如果需要现金找零功能推送现金记录里面推送过来，现场打印二维码给到车主具体打印规则和研发沟通找零业务<br>
 8:出场推送里面可以把入场部分数据一起推送了，一般情况不建议这样用，场景在于刚开始上报只推送出场没有入场数据，如果不希望出现这种情况可以本地记录每一条数据的入场是否推送，出场是否推送了，如果入场已经推送过，只推送出场部分，如果入场数据未推送可以在出场记录里面直接把入场数据和出场数据在出场接口里面一起传过来<br>
 9:入场推送的入场时间`enter_time` 和出场推送的`enter_time`值要一致，如果不一致平台可能会认为不是同一条记录，把出场推送的该条数据视为一条新的数据<br>
 10:hint返回结果里面如果有对象，比如payment_list对象返回来的数据，实际验证的时候里面值要去除转义比如 payment_list 对应的值[{\"change_value\":\"500\",\"free_value\":100,\"operator\":\"张三\",\"parking_order\":\"1624938055655\",\"pay_origin_desc\":\"现金\",\"pay_time\":\"1624938055655\",\"pay_type\":\"1\",\"value\":500}]<br>
 实际上参与签名的时候是这样的[{"change_value":"500","free_value":100,"operator":"张三","parking_order":"1624938055655","pay_origin_desc":"现金","pay_time":"1624938055655","pay_type":"1","value":500}]<br>
 原因是我们要从服务器返回来把`"`要返回来只能对它进行转义才能正常的返回前端，具体看我签名前的字符串示例

<b>数据编码</b><br/>"- 若无法正确处理UTF8编号可传递中文字段的值URLEncode后再提交, 例如提交`中文ABC`可编码为`%E4%B8%AD%E6%96%87ABC`后提交;<br/>- 这种方式提交则计算签名的值为URLEncode后的值再做签名；<br/>- 提交必须传递encoding=URL平台才会尝试URLDecode发送过来的数据。<br/><br/>


### 请求参数

| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :--------------------- | :----------------------------------------------------------- | :----: | :--: | :----------------------------------------------------------- |
| app_id | 平台分配的接入应用ID, 车场本地发起请求可不传递 | string | N | op1234567723122 |
| sign | 请求数据签名 | string | N | C65FCAC2D3FB5E2D3D4AD93DD20C8C39 |
| payment_list | 付记录集合, payment | string | N | [{<br>"pay_origin_desc": "微信",<br>"pay_type": 2,<br>"operator": "张三",<br>"pay_time": "1572410847545",<br>"value": 500,<br>"free_value": 100,<br>"parking_order": "PO201910301247278862",<br>"change_value": 100<br>}] |
| park_uuid | 平台分配的停车场UUID | string | Y | PARKUUID-XXXX-XXX-XXX |
| api_type | 接口类型配合app_id使用不传app_id的不传该字段 | string | Y | XXX-SERVICE |
| parking_serial | 停车场端的停车流水, 一般为入场记录ID | string | Y | PARKINGSERIAL-123456789 |
| plate | 车牌号码 | string | N | 粤B12345 |
| plate_color | 车牌颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, -1 未知 | string | Y | 1 |
| plate_type | 车牌类型: 0-中国大陆车牌, 1-境外车牌(含港澳), 2-两轮电动车车牌 | int | N | 1 |
| enter_time | 入场时间, 单位ms | string | Y | 1552976318722 |
| enter_image | 入场图片路径, 任意路径 | string | N | https://files.4pyun.com/d/123566 |
| enter_image_file | 入场图片文件 | string | N | 字节流 |
| enter_image_hash | 入场图片文件MD5哈希, 传递`enter_image_file`时候必须传递上传文件的哈希值。 | string | N | XXXXXXX |
| enter_plate_image | 入场车牌图片路径, 任意路径 | string | N | https://files.4pyun.com/d/123566 |
| enter_plate_image_file | 入场图片文件 | string | N | 二进制文件流 |
| enter_plate_image_hash | 入场车牌图片文件MD5哈希, 传递`enter_plate_image_file`时候必须传递上传文件的哈希值。 | string | N | XXXXXXX |
| enter_gate | 入口名称 | string | N | 东门入口 |
| enter_security | 入口保安 | string | N | 张三 |
| car_type | 车类: 1.临停车辆, 2.月卡车辆, 3.贵宾车辆(免费车), 4.储值车辆, 0.其他未知 | string | Y | 1 |
| car_desc | 车类描述 | string | Y | 临停/月卡A |
| car_color | 车辆颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, 6.银色, 7. 灰色, 8. 橙色, -1 未知 | string | N | 1 |
| vehicle_type  | 车型: 1.小车, 2.大车, 3.电动自行车/摩托车, -1.未知 | string |  N   | 1  |
| charge_type   | 车辆计费标准 | string |  Y   | 1  |
| leave_time | 离场时间, 单位ms | string | Y | 1552976318722 |
| leave_image | 离场图片路径 | string | N | https://files.4pyun.com/d/123566 |
| leave_image_file | 离场图片文件 | string | N | 二进制文件流 |
| leave_image_hash | 离场图片文件MD5哈希, 传递`leave_image_file`时候必须传递上传文件的哈希值。 | string | N | 37693CFC748049E45D87B8C7D8B9AACD
| leave_plate_image | 离场车牌图片路径, 任意路径 | string | N | https://files.4pyun.com/d/123566 |
| leave_plate_image_file | 离场车牌图片文件 | string | N | 二进制文件流 |
| leave_plate_image_hash | 离场车牌图片文件MD5哈希, 传递`leave_plate_image_file`时候必须传递上传文件的哈希值。 | string | N | 37693CFC748049E45D87B8C7D8B9AACD |
| leave_image_file | 离场图片文件 | string | N | 二进制文件流 |
| leave_plate_image_file | 离场车牌图片文件 | string | N | 二进制文件流 |
| leave_gate | 出口名称 | string | N | 西门出口 |
| leave_security | 出口保安 | string | N | 李四 |
| total_value | 总停车总费用, 单位分 | string | N | 1000 |
| free_value | 总优惠总额, 单位分 | string | N | 1000 |
| online_value | 在线支付总额, 单位分 | string | N | 1000 |
| balance_value | 储值车扣费总额, 单位分 | string | N | 1000 |
| cash_value | 现金支付总额, 单位分 | string | N | 1000 |
| prepaid_value | 先出后扣金额, 单位分 | string | N | 1000 |
| idcard_name | 通行证件姓名 | string | N | 阿Q |
| idcard_no | 通行证件号码 | string | N | 9527123 |
| idcard_image | 通行证件图片 | string | N | https://a.b.c/d.jpg |
| enter_release_reason | 入场人工放行原因 | string | N | 访问客户 |
| leave_release_reason | 离场人工放行原因 | string | N | 免费通行 |
| total_parking_space | 车场总车位数 | string | N | 100 |
| remain_parking_space | 剩余可用车位数, 仅当`total_parking_space>0`生效! | string | N | 10 |

<br>

payment字段说明

| 字段名称 | 字段说明 | 类型 | 必填 | 示例 |
| :-------------- | :----------------------------------------------------- | :----: | :--: | :------------------------------------------------------- |
| pay_origin_desc | 微信 | string | N | 到账说明 |
| pay_type | 支付类型: 1现金, 2在线支付, 3无感支付, 4储值卡余额支付 | string | N | 2 |
| operator | 收费员 | string | N | 张三 |
| pay_time | 付款时间戳, 单位ms | string | N | 1572410847545 |
| value | 实收(不含优惠金额), 单位分 | string | N | 1572410847545 |
| free_value | 优惠金额, 单位分 | string | N | 1572410847545 |
| parking_order | 车场订单号必须唯一, 相同重复上报不受理 | string | N | PO201910301247278862 |
| change_value | 1572410847545 | string | N | 找零金额(=收现金总额 - 实收), 单位分, 仅现金缴费传递生效 |


### 请求示例


```
带文件上传(不带图片的参考入场推送不带hash值的写法)
签名前字符串
1.1：签名前字符串
 str=car_color=1&car_desc=临时车&car_type=1&enter_time=1624874732253&leave_gate=出口名称&leave_image=https://t7.baidu.com/it/u=1595072465,3644073269&fm=193&f=GIF&leave_image_hash=3EF48C1A8A95DF308EFF872082280815&leave_release_reason=离场人工放行原因&leave_security=出口保安&leave_time=1624937505093&park_uuid=49f0cc52-e8c7-41e3-b54d-af666b8cc11a&parking_serial=202106028000000007&payment_list=[{"change_value":"500","free_value":100,"operator":"张三","parking_order":"1624937505093","pay_origin_desc":"现金","pay_time":"1624937505093","pay_type":"1","value":500},{"change_value":"0","free_value":100,"operator":"","parking_order":"1624937505193","pay_origin_desc":"在线支付","pay_time":"1624937505093","pay_type":"2","value":500},{"change_value":"0","free_value":0,"operator":"","parking_order":"1624937505193","pay_origin_desc":"储值卡余额支付","pay_time":"1624937505093","pay_type":"4","value":500}]&plate=粤X44444&plate_color=1&remain_parking_space=10&total_parking_space=100&vehicle_type=1&app_secret=123

1.2：MD5(str)
 sign=142FA4A7E91F18E7545328900498933B

```

```
带文件上传
签名前字符串
1.1：签名前字符串
 str=car_color=1&car_desc=临时车&car_type=1&enter_gate=入口名称&enter_image=https://t7.baidu.com/it/u=1595072465,3644073269&fm=193&f=GIF&enter_image_hash=3EF48C1A8A95DF308EFF872082280815&enter_release_reason=访问客户&enter_security=入口保安&enter_time=1624874732253&park_uuid=49f0cc52-e8c7-41e3-b54d-af666b8cc11a&parking_serial=202106028000000007&plate=粤X44444&plate_color=1&remain_parking_space=10&total_parking_space=100&vehicle_type=1&app_secret=123
1.2：MD5(str)
 sign=F58EDB51E299C99BE0E24D84739994CA


@Test
 public void testLeave() {

 try{
 String fileName = "/Users/zhichaoluo/Desktop/abcde.jpeg";
 String md5 = MD5Util.md5HashCode32(fileName);
 // 自动对key进行排序
 TreeMap<String, String> map = new TreeMap<>();
 // 平台分配的停车场UUID 实际场景读配置 必传
 map.put("park_uuid", "49f0cc52-e8c7-41e3-b54d-af666b8cc11a");
 // 停车场端的停车流水, 一般为入场记录ID 必传
 map.put("parking_serial", "202106028000000007");
 map.put("plate", "粤X44444");
 map.put("plate_color", "1");
 // 入场时间 , 单位ms 必传
 map.put("enter_time", 1624874792253l - 1000 * 60 * 1 + "");
 // 入场图片
 map.put("leave_image","https://t7.baidu.com/it/u=1595072465,3644073269&fm=193&f=GIF");
 map.put("leave_gate","出口名称");
 map.put("leave_security","出口保安");
 // 车类: 1.临停车辆, 2.月卡车辆, 3.贵宾车辆(免费车), 4.储值车辆, 0.其他未知 必传
 map.put("car_type", "1");
 // 车类描述 必传
 map.put("car_desc", "临时车");
 // 车辆颜色: 1.蓝色, 2.黄色, 3.白色, 4.黑色, 5.绿色, 6.银色, 7. 灰色, 8. 橙色, -1 未知
 map.put("car_color", "1");
 // 车型: 1.小车, 2.大车, -1.未知
 map.put("vehicle_type", "1");
 // 入场人工放行原因 例如访客访问
 map.put("leave_release_reason","离场人工放行原因");
 // 总车位
 map.put("total_parking_space","100");
 // 剩余车位 如果希望传出入场的时候顺便车位数也同步了可以这里传
 map.put("remain_parking_space","10");
 // 图片hash 传了这个值一定要传文件
 map.put("leave_image_hash", md5.toUpperCase());
 // 出场时间
 map.put("leave_time", System.currentTimeMillis() + "");
 List<Map<String,Object>> paymentList = new ArrayList<>();
 // 现金数据
 Map<String,Object> map1 = new TreeMap<>();
 map1.put("pay_origin_desc","现金");
 // 支付类型: 1现金, 2在线支付, 3无感支付, 4储值卡余额支付
 map1.put("pay_type","1");
 // 收费员
 map1.put("operator","张三");
 // 支付时间
 map1.put("pay_time",System.currentTimeMillis()+"");
 // 实收(不含优惠金额), 单位分
 map1.put("value",500);
 // 优惠金额现金优惠, 单位分
 map1.put("free_value",100);
 // 本地停车场系统订单号
 map1.put("parking_order",System.currentTimeMillis()+"");
 // 找零金额5元
 map1.put("change_value",500+"");
 // 移动支付数据
 Map<String,Object> map2 = new TreeMap<>();
 // 线上支付部分
 map2.put("pay_origin_desc","在线支付");
 // 支付类型: 1现金, 2在线支付, 3无感支付, 4储值卡余额支付
 map2.put("pay_type","2");
 // 收费员
 map2.put("operator","");
 // 支付时间
 map2.put("pay_time",System.currentTimeMillis()+"");
 // 实收(不含优惠金额), 单位分
 map2.put("value",500);
 // 优惠金额线上优惠, 单位分
 map2.put("free_value",100);
 // 本地停车场系统订单号
 map2.put("parking_order",System.currentTimeMillis()+100+"");
 // 找零金额0元
 map2.put("change_value",0+"");
 // 储值卡支付
 Map<String,Object> map4 = new TreeMap<>();
 // 线上支付部分
 map4.put("pay_origin_desc","储值卡余额支付");
 // 支付类型: 1现金, 2在线支付, 3无感支付, 4储值卡余额支付
 map4.put("pay_type","4");
 // 收费员
 map4.put("operator","");
 // 支付时间
 map4.put("pay_time",System.currentTimeMillis()+"");
 // 实收(不含优惠金额), 单位分
 map4.put("value",500);
 // 优惠金额, 单位分
 map4.put("free_value",0);
 // 本地停车场系统订单号
 map4.put("parking_order", System.currentTimeMillis() + 100 + "");
 // 找零金额0元
 map4.put("change_value",0+"");

 paymentList.add(map1);
 paymentList.add(map2);
 paymentList.add(map4);

 map.put("payment_list", JSON.toJSONString(paymentList));
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
 MultipartEntityBuilder builder1 = MultipartEntityBuilder.create()
 .setMode(HttpMultipartMode.BROWSER_COMPATIBLE)
 .setCharset(Charset.forName("utf-8"));

 for (String key : map.keySet()) {
 ContentType contentType = ContentType.create(HTTP.PLAIN_TEXT_TYPE, HTTP.UTF_8);
 StringBody stringBody = new StringBody(map.get(key),contentType);
 builder1.addPart(key, stringBody);
 }
 builder1.addTextBody("sign", sign);
 builder1.addBinaryBody("leave_image_file", new File(fileName));
 //创建表单
 Response response = Request.Post("https://api.4pyun.com/gate/1.0/parking/internal/leave")
 .body(builder1.build())
 .execute();
 HttpResponse response1 = response.returnResponse();
 System.out.println(response1.getStatusLine());
 String text = IOUtils.toString(response1.getEntity().getContent(), "utf-8");
 System.out.println(text);
 } catch (IOException e) {
 e.printStackTrace();
 }
 }catch (Exception e){
 e.printStackTrace();
 }
 }

```

### 请求返回结果参数说明
| 字段名称 | 字段说明 | 类型 | 必填 | 备注 |
| :------- | :------------- | :----: | :--: | :----------------------------------------------------------- |
| code | 请求状态码 | string | Y | 200:成功受理<br> 400:参数错误<br> 403:访问被拦截<br>500:服务器内部错误<br>503:服务暂不可用 |
| message | 返回描述 | string | Y | 返回描述 |
| hint | 返回错误说明 | string | N | 返回具体错描述指导 |
| seqno | 服务器日志标示 | string | Y | 查日志用到查问题尽量提供这个值 |


### 请求返回结果示例

```
正常返回
{
	"code": "200",
	"message": "OK",
	"seqno": "47ac4e615fa7d439",
	"data_node": "CN-South/HS3-3"
}
```

```
正常返回 带hint 对接过程中要特别注意这种情况，正式上线了这种也直接视为成功，防止一直卡在一条数据上面
{
	"code": "200",
	"message": "已忽略当前请求",
	"hint": "签名验证不通过[car_color=1&car_desc=临时车&car_type=1&enter_time=1624874732253&leave_gate=出口名称&leave_image=https://t7.baidu.com/it/u=1595072465,3644073269&fm=193&f=GIF&leave_image_hash=3EF48C1A8A95DF308EFF872082280815&leave_release_reason=离场人工放行原因&leave_security=出口保安&leave_time=1624938055655&park_uuid=49f0cc52-e8c7-41e3-b54d-af666b8cc11a&parking_serial=202106028000000007&payment_list=[{\"change_value\":\"500\",\"free_value\":100,\"operator\":\"张三\",\"parking_order\":\"1624938055655\",\"pay_origin_desc\":\"现金\",\"pay_time\":\"1624938055655\",\"pay_type\":\"1\",\"value\":500},{\"change_value\":\"0\",\"free_value\":100,\"operator\":\"\",\"parking_order\":\"1624938055755\",\"pay_origin_desc\":\"在线支付\",\"pay_time\":\"1624938055655\",\"pay_type\":\"2\",\"value\":500},{\"change_value\":\"0\",\"free_value\":0,\"operator\":\"\",\"parking_order\":\"1624938055755\",\"pay_origin_desc\":\"储值卡余额支付\",\"pay_time\":\"1624938055655\",\"pay_type\":\"4\",\"value\":500}]&plate=粤X44444&plate_color=1&remain_parking_space=10&total_parking_space=100&vehicle_type=1&app_secret=***]",
	"seqno": "6542ed9c3cbb7905",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/parking/internal/leave"
}
```

```
参数错误
{
	"code": "400",
	"message": "请求参数错误",
	"hint": "参数`enter_time`未传递",
	"seqno": "d2d0d0dab20dc6c0",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/parking/internal/leave"
}
```

```
// 服务器内部错误
{
	"code": "500",
	"message": "服务器内部错误",
	"seqno": "48bf04ea4caa479c",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/parking/internal/leave"
}
```

```
服务器内部错误
{
	"code": "503",
	"message": "服务暂不可用",
	"seqno": "48bf04ea4caa479c",
	"data_node": "CN-South/HS3-2",
	"path": "POST /gate/1.0/parking/internal/leave"
}
```
