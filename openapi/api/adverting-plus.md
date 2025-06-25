# 会员券包产品接入指南

本文档介绍如何接入会员券包产品的支付流程，并在支付按钮点击时添加广告拦截逻辑。

---

## 接入流程

### 1. 广告拦截逻辑

在支付按钮点击时，需调用广告拦截逻辑 `pyun.advertingIntercept`。根据拦截回调的 `event.action` 值，执行不同的业务逻辑：

- **`REDIRECT`**：用户确认购买，业务方需下单并通过广告组件完成支付跳转。
- **其他值**：用户取消购买，业务方继续自己的支付流程。

**代码示例：**

```javascript
// STEP-1: 拦截广告流程
if (pyun) {
    pyun.advertingIntercept((event) => {
        if (event.action === 'REDIRECT') {
            // 用户确认购买
            alert(`ACTION=${event.action}, 用户确认购买，业务方下单后由广告组件跳转完成业务支付和广告产品购买!`);
        } else {
            // 用户取消购买
            alert(`ACTION=${event.action}, 用户取消购买，业务方继续自己的支付流程即可!`);
        }
        this.doRealPay(event);
    });
} else {
    // 说明JS SDK未正确加载，需检查原因，业务方继续自己的支付流程即可!
    this.doRealPay(event);
}
```

---

### 2. 支付逻辑实现

在广告拦截回调中，调用 `doRealPay` 方法完成支付逻辑。以下是支付逻辑的实现示例：

```javascript
function doRealPay(event) {
    // 调用支付接口
    payment((reply) => {
        /**
         * 参数说明（以下参数二选一）：
         * - `depute_serial`：托收订单编号
         * - `depute_id`：托收订单ID
         */
        let args = {
            depute_serial: reply.payload.pay_serial,
        };

        // STEP-2: 根据拦截结果跳转支付
        if (event && event.action === 'REDIRECT') {
            event.redirect(args); // 广告组件跳转支付
        } else {
            // 正常支付流程
            console.log('正常跳转支付');
        }
    });
}
```

---

## 方法说明

广告事件拦截（如会员券包购买拦截弹窗）。

**方法签名：**
```javascript
pyun.advertingIntercept(callback, enforce)
```

**参数说明：**

- `callback` (Function)：拦截回调函数，参数为对象，常见字段如下：
  - `action`：'REDIRECT'（跳转购买）或 'NONE'（放弃）
  - `redirect`：跳转方法（当 action 为 'REDIRECT' 时可用）
- `enforce` (Boolean)：是否强制执行拦截（可选，默认 false）

**返回值：**
- 无

**使用示例：**
```javascript
pyun.advertingIntercept(function(res) {
  if (res.action === 'REDIRECT') {
    // 跳转购买
    res.redirect({depute_id: 'xxx'});
  } else {
    // 用户放弃
    console.log('用户放弃购买');
  }
});
```

---

## 注意事项

1. **拦截回调处理**：
   - 确保在 `event.action === 'REDIRECT'` 时调用 `event.redirect(args)`，完成支付跳转。
   - 其他情况下，需根据业务需求处理支付流程。

2. **参数校验**：
   - `depute_serial` 和 `depute_id` 必须二选一传递，确保支付接口调用成功。
   - `depute_id`取[交易预请求](./trade-prepare.html)返回的`pay_id`子弹

3. **用户体验**：
   - 在支付流程中，建议添加加载提示或进度条，提升用户体验。

---

## 示例场景

- 用户点击支付按钮后，广告拦截逻辑会判断用户是否确认购买。
- 如果用户确认购买，广告组件将跳转至支付页面完成支付。
- 如果用户取消购买，业务方可继续执行自己的支付流程。

---

以上是会员券包产品接入的完整流程。如有疑问，请联系技术支持。