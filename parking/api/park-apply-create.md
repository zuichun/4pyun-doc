# 提交车场进件

### 省市区参数
本文中的接口凡涉及省市区参数的，分位两类传递：

1. 传递省市区编码，编码参考附件“**area_code.json**”
2. 传递省市区名称，名称参考同上

参数具体传递类型，可参考参数说明及示例。

关于附件"**area_code.json**"说明：

该文件为JSON格式的省市区树形结构数据，其中`code`字段即为我们需要的编码，`type`类型字段定义为：

1. 省份
2. 城市
3. 地区

### 2.3 银行参数
本文中接口的银行参数主要包括：

1. 银行名称
2. 支行名称
3. 银行所在省份（传递名称）
4. 银行所在城市（传递名称）

以上四个参数具有关联性，且银行名称，支行名称需在附件“**银行信息列表_20200603.xls**”所列的银行名单内。

### 2.4 时间格式
P云接口需要传递日期/时间的参数，统一采用**UTC时间格式**，即“**yyyy-MM-ddTHH:mm:ssZ**”这种格式，例如：2018-12-10T16:00:00Z

对于身份证以及营业执照一类需要表示**长期有效**的截止时间字段，我们约定传参数“**LONG_TERM**”

## 3. 停车场进件接口：

### 3.1 停车场创建接口
*https://api.4pyun.com/gate/1.0/park/apply*

- 说明：创建停车场并配置支付渠道，如仅创建车场，可不传支付渠道配置相关参数，支付渠道可在P云管理平台提交申请配置。

- 请求方法：POST

- 请求参数：

|参数名|必填|说明|类型|示例|
|:----|:--|:--|:---|:---|
|app_id |是|平台分配应用ID|string|op72****24 |
|merchant |是|平台分配厂商商户号|string|PV3***4 |
|tech_code|是|平台分配设备软件编码|string|PO2**1|
|park_name|是|车场名称，在平台中要求唯一，不可重复|string|测试-停车场|
|province_code|是|省份编码|string|440000|
|city_code|是|城市编码|string|440300|
|district_code|是|区/县编码|string|440305|
|address|是|详细地址|string|讯美科技广场|
|total\_parking\_space|是|总车位数|int32|50|
|charge_image|否|收费牌照片|string|http://www.660pp.com/img.jpg|
|enter\_gate\_image|否|入口图片集合|string|http://www.660pp.com/img1.jpg|
|leave\_gate\_image|否|出口图片集合|string|http://www.660pp.com/img1.jpg|
|park\_image|否|停车场照片集合|string|http://www.660pp.com/img1.jpg|
|distributor_code|否|关联渠道商商户号|string|PC597835|
|sub\_distributor\_code|否|关联子渠道商商户号|string|PS597735|
|estate_code|否|所属物业商户号|string|PR507835|
|manager_name|是|联系人名称（用于创建管理员）|string|\*丁华|
|manager_telephone|是|联系电话（用于创建管理员账号密码））|string|13600000162|
|service_phone|是|客服电话|string|0755-28823700|
|service\_phone\_status|否|客服电话核验状态|int32|0: 未核验；1:已核验|
|api\_park\_uuid|否|第三方停车场Id|string|TD1232423|
|api_key|否|第三方通信Key|string|DO29h38FSDF|
|apply\_channel\_type|是|配置支付渠道类型，目前暂时支持类型：5，即P云聚合支付，若仅创建车场不配置支付渠道，可传：0，支付渠道可在P云管理平台创建；|int32|5|
|bank_account|否|结算账户配置参数，若channel_type为5，该项必填|object||
|business_licence|否|营业执照信息，若account_type为0，该项必填|object||
|id_card|是|收款人身份信息，若account_type为1，填写收款人身份证；若account_type为0，填写法人身份证；|object||
|contact_person|否|电子合同签约代表联系人，若account_type为0，该项必填|Object||

|bank_account|必填|说明|类型|示例|
|:----|:--|:--|:---|:---|
|account_type|是|银行账户类型|int32|0: 对公；1: 对私|
|account_name|是|银行账户名称|string|深圳市神州路通技术有限公司|
|account_bank|是|银行名称|string|平安银行|
|account_branch|是|支行名称|string|平安银行深圳大冲支行|
|account_number|是|银行账号|string|1500\*\*\*\*255713|
|account_province|是|银行所在省份名称|string|广东省|
|account_city|是|银行所在城市名称|string|深圳市|
|account_telephone|否|对私账户（account_type为1）必填，银行预留手机号| 13800138000|

|business_licence|必填|说明|类型|示例|
|:----|:--|:--|:---|:---|
|business_name|是|商户名称|string|深圳市神州路通技术有限公司|
|business\_licence\_img|是|营业执照照片|string|http://rrd.me/gkQGC|
|business\_licence\_no|是|营业执照编号|string|***|
|business\_licence\_effect|是|营业执照生效时间|string|2018-12-10T16:00:00Z|
|business\_licence\_expire|是|营业执照到期时间，格式同生效时间，如长期有效，传: LONG_TERM |string|LONG_TERM|
|business\_register\_address|是|营业执照注册地址|string|深圳市圳市南山区粤海街道科技园社区科苑路8号讯美科技广场3号楼1013|
|business\_legal\_person|是|法人|string|李\*\*|

|id_card|必填|说明|类型|示例|
|:----|:--|:--|:---|:---|
|no|是|证件号码|string|370702196208022615|
|name|是|证件姓名|string|李四|
|img_front|是|证件照片正面图片地址|string|http://rrd.me/gkQGC|
|img_back|是|证件照片反面图片地址|string|http://rrd.me/gkQGC|
|effect_date|是|证件生效时间|string|2018-12-10T16:00:00Z|
|expire_date|是|证件到期时间|string|2018-12-10T16:00:00Z|
|type|是|证件类型|int32|证件类型：1. 身份证|

| contact_person | 必填 | 说明       | 类型   | 示例        |
| :------------- | :--- | :--------- | :----- | :---------- |
| name           | 是   | 联系人姓名 | string | 李四        |
| telephone      | 是   | 联系人电话 | string | 18220283762 |


- 请求示例：

 ```json
{
	"address":"讯美科技广场",
	"api_key":"D127h16FSDF",
	"api_park_uuid":"TD1232423",
	"app_id":"op02759c5493c7bf6",
	"apply_channel_type":"5",
	"bank_account":{
		"account_number":"283487234234",
		"account_type":"0",
		"account_branch":"中国银行深圳市分行",
		"account_name":"丁华娱乐有限公司",
		"account_bank":"中国银行",
		"account_province":"广东省",
		"account_city":"深圳市"
	},
	"business_licence":{
		"business_name":"丁华娱乐有限公司",
		"business_legal_person":"万丁华",
		"business_licence_expire":"LONG_TERM",
		"business_licence_img":"https://files.4pyun.com/d/80eaf09a9c",
		"business_licence_no":"PK124234234",
		"business_licence_effect":"2001-01-01T00:00:00Z",
		"business_register_address":"广东省深圳市讯美科技广场"
	},
	"city_code":"440300",
	"district_code":"440305",
	"manager_name":"徐先生",
	"manager_telephone":"15914040963",
	"park_name":"测试_XZF16",
	"province_code":"440000",
	"total_parking_space":"5"
}

 ```


- 返回参数：

|payload参数名|说明|类型|示例|
|:--|:--|:--|:--|
|park_uuid|停车场UUID|string|45d789f1-c4d3-45aa-ace3-91a8fe161120|
|park_code|停车场商户号|string|38757657|
|manager_account|管理员账号信息|object||

|manager_account|说明|类型|示例|
|:--|:--|:--|:--|
|login_url|账号登录网站链接|string|https://mch.4pyun.com|
|name|管理员姓名|string|\*丁华|
|merchant|商户号|string|38757657|
|account|管理员账号|string|13600000162|
|password|管理员密码|string|13600000162|

- 返回示例

 ```json
{
	"code":"1001",
	"data_node":"ChinaRoad-007",
	"seqno":"Hnms4EmUgFZ18TGm",
	"payload":{
		"parkUuid":"e06757cf-5694-4111-b2b3-269fa6c68f28",
		"manager_account":{
			"login_url":"https://mch.4pyun.com",
			"password":"15914040963",
			"name":"徐先生",
			"merchant":"22803716",
			"account":"15914040963"
		},
		"park_code":"22803716"
	}
}

 ```

### 3.2 停车场查询接口
*https://api.4pyun.com/gate/1.0/park/apply*

- 说明：查询停车场信息，包含支付渠道配置信息

- 请求方法：GET

- 请求参数：

|参数名|必填|说明|类型|示例|
|:----|:--|:--|:---|:---|
|app_id |是|平台分配应用ID|string|op72****24 |
|merchant |是|平台分配厂商商户号|string|PV3***4 |
|park_uuid|是|平台分配停车场UUID|string|45d789f1-c4d3-45aa-ace3-91a8fe161120|

- 请求示例：

 ```
 https://api.4pyun.com/gate/1.0/park/apply?app_id=op72****24&park_uuid=e24deadf-1aa0-4643-bde5-f9c474c4f5f5&sign=D536C1408BE54554765B26A96126E62D
 ```


- 返回参数：

|payload参数名|说明|类型|示例|
|:--|:--|:--|:--|
|park_uuid|停车场UUID|string|45d789f1-c4d3-45aa-ace3-91a8fe161120|
|park_name|停车场名称|string|测试-停车场|
|supplier|厂商信息|object||
|distributor|渠道商信息|object||
|sub_distributor|子渠道商信息|object||
|estate_management|物业信息|object||
|total\_parking\_space|总车位数|int32|50|
|manager_name|管理员名称|string|*丁华|
|manager_telephone|管理员联系方式|string|13600000162|
|province_code|停车场所在省份编码|string|440000|
|city_code|停车场所在城市编码|string|440300|
|district_code|停车场所在地区编码|string|440305|
|address|停车场所在地址|string|广东省深圳市南山区讯美科技广场|
|enter\_gate\_image|车场入口照片|string|http://www.660pp.com/img1.jpg|
|leave\_gate\_image|车场出口照片|string|http://www.660pp.com/img1.jpg|
|park_image|车场场内照片|string|http://www.660pp.com/img1.jpg|
|charge_image|车场收费牌照片|string|http://www.660pp.com/img1.jpg|
|park_code|停车场商户号|string| 38757657 |
|applicant|停车场申请人商户号|string|PV868691|
|api\_park\_uuid|第三方停车场Id|string|TD1232423|
|api_key|第三方通信Key|string|DO29h38FSDF|
|tech_provider|技术厂商信息|object||
|channel\_type\_config|支付渠道配置集|array(object)||
|channel_config|支付方式配置集（目前该参数暂无实际意义，支付渠道配置状态请参考channel\_type\_config）|array(object)||

| supplier / distributor/ sub\_distributor / estate_management |说明|类型|示例|
|:--|:--|:--|:--|
|merchant|商户号|string|PO6277|
|name|商户名称|string|TD智能|
|type|商户类型|int32|2|

| tech_provider |说明|类型|示例|
|:--|:--|:--|:--|
|code|技术厂商编码|string|PO6277|
|name|厂商名称|string|TD智能|
|status|可用状态：1，可用；0，不可用|int32|1|
|alias|别名|string|TD|
|require_param|参数配置类型: 0,无需额外配置参数；1，需要api\_park\_uuid; 2, 需要api\_park\_uuid和api_key|int32|1|

| channel\_type\_config |说明|类型|示例|
|:--|:--|:--|:--|
|type_id|支付渠道类型Id|int32|5|
|name|支付渠道名称|string|P云聚合支付|
|status|开通状态, -3，已失效；-2，审核不通过；0，未开通；1，待审核； 2，审核中； 3，审核通过|int32|3|
|abnormal|异常状态, 0，正常；1，账户异常（聚合支付账户）|int32|0|
|apply_no|渠道申请编号|string|C202003028360|
|apply\_sign\_url|支付渠道申请签约链接(仅微信对公使用)|string||

| channel\_config |说明|类型|示例|
|:--|:--|:--|:--|
|type_id|支付渠道类型Id|int32|5|
|name|支付渠道名称|string|P云聚合支付|
|channel|支付方式集|array(object)|

| channel |说明|类型|示例|
|:--|:--|:--|:--|
|type_id|所属支付渠道类型Id|int32|5|
|name|支付方式名称|string|微信扫码|
|channel_code|支付方式编码|string|200201|
|pay_mode|到账方式|string|1|
|status|支付方式开通状态|string|1|

- 返回示例

 ```json
 {
	"code":"1001",
	"data_node":"HS1-1",
	"seqno":"d98b78fcc8c60ce3",
	"payload":{
		"enter_gate_image":"",
		"park_image":"",
		"address":"广东省深圳市南山区讯美科技广场",
		"leave_gate_image":"",
		"city_code":"440300",
		"park_code":"70552772",
		"manager_telephone":"15914040963",
		"province_code":"440000",
		"park_name":"测试_XZF14-停车场",
		"channel_config":[
			{
				"type_id":1,
				"name":"微信对公",
				"channel":[
					{
						"pay_mode":3,
						"type_id":1,
						"name":"扫码支付",
						"channel_code":"200201",
						"status":0
					},
					{
						"pay_mode":3,
						"type_id":1,
						"name":"微信车主服务",
						"channel_code":"300005",
						"status":0
					}
				]
			},
			{
				"type_id":2,
				"name":"微信对私",
				"channel":[
					{
						"pay_mode":11,
						"type_id":2,
						"name":"扫码支付",
						"channel_code":"200201",
						"status":0
					}
				]
			},
			{
				"type_id":3,
				"name":"支付宝对公",
				"channel":[
					{
						"pay_mode":4,
						"type_id":3,
						"name":"扫码支付",
						"channel_code":"100102",
						"status":0
					},
					{
						"pay_mode":4,
						"type_id":3,
						"name":"支付宝车主服务",
						"channel_code":"300004",
						"status":0
					}
				]
			},
			{
				"type_id":4,
				"name":"支付宝对私",
				"channel":[
					{
						"pay_mode":12,
						"type_id":4,
						"name":"扫码支付",
						"channel_code":"100102",
						"status":0
					}
				]
			},
			{
				"type_id":5,
				"name":"P云聚合支付",
				"channel":[
					{
						"pay_mode":1,
						"type_id":5,
						"name":"微信扫码",
						"channel_code":"200201",
						"status":-1
					},
					{
						"pay_mode":1,
						"type_id":5,
						"name":"微信无感",
						"channel_code":"300005",
						"status":-1
					},
					{
						"pay_mode":1,
						"type_id":5,
						"name":"支付宝扫码",
						"channel_code":"100102",
						"status":-1
					},
					{
						"pay_mode":1,
						"type_id":5,
						"name":"支付宝无感",
						"channel_code":"300004",
						"status":-1
					}
				]
			}
		],
		"applicant":"P云测试厂商",
		"channel_type_config":[
			{
				"apply_sign_url":"",
				"type_id":1,
				"apply_no":"",
				"name":"微信对公",
				"status":0
			},
			{
				"apply_sign_url":"",
				"type_id":2,
				"apply_no":"",
				"name":"微信对私",
				"status":0
			},
			{
				"apply_sign_url":"",
				"type_id":3,
				"apply_no":"",
				"name":"支付宝对公",
				"status":0
			},
			{
				"apply_sign_url":"",
				"type_id":4,
				"apply_no":"",
				"name":"支付宝对私",
				"status":0
			},
			{
				"apply_sign_url":"",
				"type_id":5,
				"apply_no":"C202002287363",
				"name":"P云聚合支付",
				"status":3
			}
		],
		"manager_name":"徐先生",
		"api_key":"DO29h38FSDF",
		"district_code":"440305",
		"api_park_uuid":"TD1232423",
		"supplier":{
			"name":"P云测试厂商",
			"merchant":"PV376764",
			"type":2
		},
		"total_parking_space":5,
		"charge_image":"",
		"tech_provider":{
			"code":"PO6277",
			"require_param":1,
			"name":"腾达智能",
			"alias":"",
			"status":1
		}
	}
}
 ```

## 4. 支付渠道申请链接：
### 4.1 链接传参说明

**跳转链接示例**：https://mch.4pyun.com/external/payment/channel/config?merchant=xxx&park_code=xxx&token=xxx

**说明**：跳转链接前需保证车场已在平台创建(调用[3.1 停车场创建接口](#3.1))

**URL**: https://b.4pyun.com/external/channel/list

**参数**：

- merchant: 平台分配厂商编码

- park_code: P云平台车场商户号(调用[3.1 停车场创建接口](#3.1)返回park_code参数)

- token: 请求令牌

<h1 id=4.2></h1>
### 4.2 令牌生成规则

```javascript
let content = JSON.stringify({
                iss: 'op01958c1095c8bf4', // 应用ID
                iat: (+new Date() / 1000).toFixed(0), // 令牌生成时间戳(单位秒)
                exp: ((+new Date() + 24 * 60 * 60 * 1000) / 1000).toFixed(0), // 令牌过期时间(单位秒)
            })
let secret = "79B0F3E6E956A6F272D9E74FABD95EDE"; //应用ID密钥
let signature = MD5.hashBinary(content + "&key=" + secret); // 生成签名
let token = "Basic " + Base64.encode(content) + "." + signature; //生成令牌(注意"Basic"后有一空格)
token = encodeURI(token) // UrlEncode
```

## 5. ~~支付渠道申请链接(老平台，即将废弃)：~~
### 5.1 链接传参说明

**跳转链接示例**：https://b.4pyun.com/external/channel/list?app\_id=op01958c1095c8bf4&merchant=PV123422&api\_park\_uuid=WDH123456&token=Basic%20eyJpc3MiOiJvcDAxOTU4YzEwOTVjOGJmNCIsImlhdCI6MTU5NDcwOTk1MCwiZXhwIjoxNTk0Nzk2MzUwfQ%3D%3D.99AD7461C82A43907388EE00D3801702

**说明**：跳转链接前需保证车场已在平台创建(调用[3.1 停车场创建接口](#3.1))

**URL**: https://b.4pyun.com/external/channel/list

**参数**：

- app_id: 平台分配应用ID

- merchant: 平台分配厂商编码

- api\_park\_uuid: 第三方车场ID

- token: 请求令牌

<h1 id=4.2></h1>
### 5.2 令牌生成规则

```javascript
let content = JSON.stringify({
                iss: 'op01958c1095c8bf4', // 应用ID
                iat: (+new Date() / 1000).toFixed(0), // 令牌生成时间戳(单位秒)
                exp: ((+new Date() + 24 * 60 * 60 * 1000) / 1000).toFixed(0), // 令牌过期时间(单位秒)
            })
let secret = "79B0F3E6E956A6F272D9E74FABD95EDE"; //应用ID密钥
let signature = MD5.hashBinary(content + "&key=" + secret); // 生成签名
let token = "Basic " + Base64.encode(content) + "." + signature; //生成令牌(注意"Basic"后有一空格)
token = encodeURI(token) // UrlEncode
```
