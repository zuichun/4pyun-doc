# 无感支付接入指引

无感需要你们对接:
1. 出入场推送(若对接了可忽略)，你们提供接口或者直接对接我们的接口
- https://pptingche.coding.net/s/0aaef07d-832e-4fbf-ae60-ed8c9fad6547/86
- https://pptingche.coding.net/s/0aaef07d-832e-4fbf-ae60-ed8c9fad6547/87
2. 无感扣款接口
- https://pptingche.coding.net/s/0aaef07d-832e-4fbf-ae60-ed8c9fad6547/68
3. 无感状态下发接口
- https://pptingche.coding.net/s/0aaef07d-832e-4fbf-ae60-ed8c9fad6547/67
4. 无感支付结果异步通知
- https://pptingche.coding.net/s/0aaef07d-832e-4fbf-ae60-ed8c9fad6547/66

大致流程是你推送入场给我，我会去微信判断无感状态，是无感会下发无感状态接口给你(中途用户关闭/开通也会通知)，你根据本地收到的无感状态是开启的在出口调用无感扣款接口，发起扣款。支付完成之后异步通知支付结果
