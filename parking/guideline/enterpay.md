# 付费入场接入指引

付费入场和无牌车入场同一接口交互流程

付费入场流程：
1. 车辆到入口，扫码调用“service.parking.direct.enter”
https://doc.4pyun.com/parking/api/direct-enter.html
- 若需要付费入场则返回状态码1000，并返回订单信息，必须返回parking_order和pay_value
- 若无需付费入场则按正常开闸返回1001

2. 当入场需付费平台将引导用户完成付费，付费成功后通过“service.parking.payment.result”同步缴费结果
https://doc.4pyun.com/parking/api/payment-notify.html
- 付费成功写入缴费信息
- 完成开闸放行