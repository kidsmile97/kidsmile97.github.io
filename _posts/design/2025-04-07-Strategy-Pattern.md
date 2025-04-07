---
title: 策略模式
date: 2025-04-07 20:56:43 +0800
categories: [Design Pattern]
tags: [设计模式, design pattern, 策略模式, strategy pattern]
---

# 策略模式（Strategy Pattern）【行为型】

> 策略模式的利用，是为了帮助我们更好的去拆分代码，抽象功能，实现高内聚低耦合，从而得到更好的扩展性、维护性。

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

定义一组算法，每个算法都单独封装，然后使它们之间可替换。策略模式使得该算法对于客户端使用变得非常独立。

## 通俗易懂策略模式

> 从最著名的支付案例开始

![策略模式](/assets/img/design//strategy/strategy-model1.png)

```java
void fn() {
  Order order = new Order();
  PaymentResult payRes = new PaymentResult();
  if (payType == "wechat") {
    // 微信支付流程...
    payRes = WechatPay.pay(order);
  } else if (payType == "alipay") {
    // 支付宝支付流程...
    payRes = Alipay.pay(order);
  } else if (payType == "bank") {
    // 银行卡支付流程...
    payRes = UnionPay.pay(order);
  } else {
    // 不支持的支付类型
  }
  Deliver deliver = new Deliver();
}
```

## 策略模式抽象优化

`策略模式其实是为了辅助你抽象代码，拆分解耦的一种思想`。

1. 假设需要扩展一种支付方式，即增加一个支付类
2. 修改支付策略
3. 核心业务流程 shopping 的根本不受影响，整体唯一入侵式的修改，是支付策略需要适配多一种

```java
interface Payment {
  String getType();
  Result pay(order);
}
class Wepay implements Payment {/** xxxx */}
class Alipay implements Payment {/** xxxx */}
class Bankpay implements Payment {/** xxxx */}
class PayStrategy {
  public static Payment getPayment(String payType) {/** xxx */}
}

void shopping(Payment payment) {
  Order order = new Order();
  Result res = payment.pay();
  dealRes(res);
}
```

> 可见，优化后每种支付方式独立封装，互相不限制、不影响，扩展时只需扩展新的支付方式，维护策略类，而真正的核心业务逻辑流程始终是在保护中。

这个例子用到了面向接口而非实现编程，满足了职责单一、开闭原则，从而达到了功能上的高内聚低耦合、提高了可维护性、扩展性以及代码的可读性。

## 什么时候可以使用策略模式？

1. 分支判断。对于良好设计的代码来说，if-else 出现在某个地方（比如一个函数内），这段代码肯定是为了实现某个功能，那么不同的判断条件下的 else 代码是应该存在共通性的，不然不同分支走向它实现的就是不同的功能了，所以可以抽离成策略，然后摒弃复杂、大块、难读的分支代码，通过调用、或注入的方式，使得后期扩展时也无需改动原本功能的整体逻辑。

2. 择路问题。switch-case 是固定的择路思维，那么策略模式在这里其实就相当于把 swtch-case 抽离，用一个固定的策略代替，后期只需维护策略组中不同的策略，而无需动刀原来的流程逻辑。


## 弱类型、函数式编程语言对策略模式的应用

设计模式只是一种思想，而不是面相对象的专利，请看 JavaScript 的实现例子

```js
// 一个简单的 draw 画图函数
function draw(type) {
  if (type === 'circle') {
    // use Dom api create a circle element.
    return <div class="circle">This is a circle element.</div>
  } else if (type === 'rectangle') {
    // use Dom api create a rectangle element.
    return <div class="rectangle">This is a rectangle element.</div>
  } else {
    // no this type element
    return null
  }
}
```

策略模式优化，纯函数式编程实现

```js
const circle = () => {
  // use Dom api create a circle element.
  return <div class="circle">This is a circle element.</div>
}
const rectangle = () => {
  // use Dom api create a rectangle element.
  return <div class="circle">This is a rectangle element.</div>
}
function draw(pen) {
  pen();
}
// 点击画圆
const onCircleClick = () => {
  draw(circle);
}
// 点击画矩
const onCircleClick = () => {
  draw(circle);
}
```

增加画椭圆，实现零入侵扩展

```js
const ellipse = () => {
  return <div class="ellipse">This is a ellipse element.</div>
}
// 点击画椭圆
const onCircleClick = () => {
  draw(ellipse);
}
```

> 由此可见，哪怕没有类和接口，JavaScript 由于其自由的特性完全可以达到相同的效果。

> 特别地，当改事件函数出现在 vue 或 react 等框架的组件中，这样的处理方式可以在未来扩展组件功能的同时，维持原组件内部代码的稳定性

> 确保组件整体交互功能无变化时，组件无需变更
