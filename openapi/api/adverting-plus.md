## 会员券包产品接入

1. 支付按钮点击需添加广告拦截逻辑
2. 拦截回调`event.action == 'REDIRECT'`时通过`event.redirect(args)`完成购买支付跳转

```js
// STEP-1. 拦截广告流程
pyun.advertingIntercept((event) => {
    if (event.action == 'REDIRECT') {
        // 用户确认购买
        alert('ACTION=' + event.action + ", 用户确认购买，业务方下单后由广告组建跳转完成业务支付+广告产品购买!");
        this.doRealPay(event);
    } else {
        // 用户取消购买，业务方继续自己的支付流程即可
        alert('ACTION=' + event.action + ", 用户取消购买，业务方继续自己的支付流程即可!");
        this.doRealPay();
    }
})

function doRealPay(event) {
    // 
    payment((reply) => {
        /**
         * 参数二选一
         * depute_serial - 托收订单编号
         * depute_id - 托收订单ID
         */
        let args = {
            depute_serial: reply.payload.pay_serial,
        };
        // STEP-2. 拦截调用!!!
        if (event) {
            event.redirect(args);
        } else {
            // 正常跳转支付
        }
    })
},
```