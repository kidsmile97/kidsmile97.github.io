---
---

# 策略模式（Strategy Pattern）

> 策略模式的利用，是为了帮助我们更好的去拆分代码，抽象功能，实现高内聚低耦合，从而得到更好的扩展性、维护性。

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

定义一组算法，每个算法都单独封装，然后使它们之间可替换。策略模式使得该算法对于客户端使用变得非常独立。、

### 通俗易懂策略模式

> 从 A 到 B 有 3 条路，分别是公路、小路、铁路。这时候就会有 3 种选择，使用 switch(type)-case 来做选择

- 问题 1：后来增加了地铁。此时你去增加了一个 case

策略模式思想 1：我们从 A 到 B 统一使用 go(type)，然后把选路抽离出来。现在看起来好像只是抽离了一块代码变成了函数。

- 问题 2：新需求，现在需要你把各条路走的过程写出来。你再去 go 里面把每一个路过程都写下来

策略模式思想 2：你把每条路再抽离成一个个函数，然后把各自的过程封装到函数里面。

- 问题 3：现在新增加了一个 C，又让你从 B 到 C

你发现，如果你没有经过 1、2 的优化，那么此时你可能需要写一个全新的内容去从 B 到 C，而现在，你扩展新的走路策略，只需要调用 go 即可

> `所以你知道了，策略模式其实是为了辅助你抽象代码，拆分解耦的一种思想`

### 什么时候可以使用策略模式？

1. 分支判断。对于良好设计的代码来说，if-else 出现在某个地方（比如一个函数内），这段代码肯定是为了实现某个功能，那么不同的判断条件下的 else 代码是应该存在共通性的，不然不同分支走向它实现的就是不同的功能了，所以可以抽离成策略，然后摒弃复杂、大块、难读的分支代码，通过调用、或注入的方式，使得后期扩展时也无需改动原本功能的整体逻辑。

2. 择路问题。switch-case 是固定的择路思维，那么策略模式在这里其实就相当于把 swtch-case 抽离，用一个固定的策略代替，后期只需维护策略组中不同的策略，而无需动刀原来的流程逻辑。

### 著名的支付实现样例

```java
Order order = new Order();
if (payType == 'wechat') {
  // 微信支付流程...
} else if (payType == 'alipay') {
  // 支付宝支付流程...
} else if (payType == 'bank') {
  // 银行卡支付流程...
} else {
  // 不支持的支付类型
}
```

策略模式优化

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

Order order = new Order();
Payment payment = PayStrategy.getPayment(payType);
payment.pay();
```

> 可见，优化后每种支付方式独立封装，互相不限制、不影响，扩展时只需扩展新的支付方式，维护策略类，而真正的核心业务逻辑流程始终是在保护中。

这个例子用到了面向接口而非实现编程，满足了职责单一、开闭原则，从而达到了功能上的高内聚低耦合、提高了可维护性、扩展性以及代码的可读性。

### 弱类型、函数式编程语言对策略模式的应用

设计模式只是一种思想，用 JavaScript 利用该思想进行功能开发，你且看看如何？

> 请看以下例子

```JavaScript
const onAddBtnClick = (type) => {
  if (type === 'circle') {
    // use Dom api create a circle element.
    return <div class="circle">This is a circle element.</div>
  } else if (type === 'rectangle') {
    // use Dom api create a rectangle element.
    return <div class="circle">This is a rectangle element.</div>
  } else {
    // no this type element
    return null
  }
}
```

策略模式优化

```JavaScript
const circle = () => {
  // use Dom api create a circle element.
  return <div class="circle">This is a circle element.</div>
}
const rectangle = () => {
  // use Dom api create a rectangle element.
  return <div class="circle">This is a rectangle element.</div>
}
const getPen = (type) => {
  const penMap = {
    circle: circle,
    rectangle: rectangle,
  }
  if (!penMap[type]) {
    return {
      draw: () => null;
    }
  }
  return {
    draw: pemMap[type],
  };
}
const onAddBtnClick = (type) => {
  const pen = getPen(type);
  return pen.draw();
}
```

> 由此可见，哪怕没有类和接口，JavaScript 由于其自由的特性完全可以达到相同的效果。

> 特别地，当改事件函数出现在 vue 或 react 等框架的组件中，这样的处理方式可以在未来扩展组件功能的同时，维持原组件内部代码的稳定性

> 确保组件整体交互功能无变化时，组件无需变更
