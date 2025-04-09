---
title: 设计：策略模式
date: 2025-04-07 20:56:43 +0800
categories: [Design Pattern]
tags: [设计模式, design pattern, 策略模式, strategy pattern, strategy in js]
---

# 策略模式（Strategy Pattern）【行为型】

> 策略模式的利用，是为了帮助我们更好的去拆分代码，抽象功能，实现高内聚低耦合，从而得到更好的扩展性、维护性。

Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

定义一组算法，每个算法都单独封装，然后使它们之间可替换。策略模式使得该算法对于客户端使用变得非常独立。

## 从最著名的支付案例开始

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

## 基于策略模式的抽象优化

`策略模式其实是为了辅助你抽象代码，拆分解耦的一种思想`。

```java
interface Payment {
  String getType();
  Result pay(order);
}
class Wepay implements Payment {/** xxxx */}
class Alipay implements Payment {/** xxxx */}
class Bankpay implements Payment {/** xxxx */}

void shopping(Payment payment) {
  Order order = new Order();
  Result res = payment.pay();
  dealRes(res);
}
```
优化后的扩展性：假设需要扩展一种支付方式
1. 增加一个新的支付实现类
2. 为用户提供一个使用新支付方式的入口
3. 核心业务流程 shopping 的不受任何影响，实现零代码入侵的扩展性

> 可见，优化后每种支付方式独立封装，互相不限制、不影响，扩展时只需扩展新的支付方式，维护策略类，而真正的核心业务逻辑流程始终是在保护中。

> 这里利用了面向接口而非实现编程，满足了职责单一、开闭原则，从而达到了功能上的高内聚低耦合、提高了可维护性、扩展性以及代码的可读性。

## 什么时候可以使用策略模式？

> 看完上面例子，你可能会容易注意到，策略模式应用于 if-else、switch-case 等多分支模型，用运行时的动态确认 =》替代 =》 固定分支判断代码

那只是其中一个关注点，策略模式应该有两个关注点

1. 多分支模型，无论是 if-else、switch-case、map映射、and、or、not 之类的，逻辑应该是存在`多个策略`的场景

2. `策略`能够被抽象。如果不同的分支完全没有相似性，或者说不同分支走的就是完全不应该一样的逻辑，只需要简单抽离逻辑即可。

举个例子：
```java
int main(String type) {
  int a = 100;
  int b = 10;
  switch(type) {
    case "+":
      return a + b;
    case '-':
      return a - b;
    default:
      return 0;
  }
}
// 策略模式优化
interface Operation {
  int operate(int a, int b);
}
int main(Operation oper) {
  int a = 100;
  int b = 10;
  return oper.operate(a, b);
}
```
而你是否觉得下面的代码有必要优化吗？
```java
int main(String type) {
  switch(type) {
    case "+":
      return 10 + 100;
    case '-':
      return 100 - 10;
    default:
      return 0;
  }
}
```

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

> 确保组件整体交互功能无变化时，组件无需变更，扩展的核心就是

## 常见策略模式应用

- Java JDBC：数据库链接，通过传入不同数据库的 driver 来实现对数据库连接控制，现在连驱动都可以自动加载了，本质上只需要配置驱动，完全的零入侵支持。

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
public class MySQLDemo {
    // 数据库连接信息
    private static final String URL = "jdbc:mysql://localhost:3306/your_database?useSSL=false&serverTimezone=UTC";
    private static final String USER = "root";
    private static final String PASSWORD = "your_password";

    public static void main(String[] args) throws Exception {
        //1.加载驱动程序
        Class.forName("com.mysql.jdbc.Driver");
        //2. 获得数据库连接
        Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```

## 对于例子你或许存在疑问

或许在上面的例子中你发现了，策略模式看似删除了 if-else，实际必然还有一个地方也是需要写这些代码

```java
interface Payment {
  String getType();
  Result pay(order);
}
class Wepay implements Payment {/** xxxx */}
class Alipay implements Payment {/** xxxx */}
class Bankpay implements Payment {/** xxxx */}
void shopping(Payment payment) {
  Order order = new Order();
  Result res = payment.pay();
  dealRes(res);
}
// 被忽略的使用代码
void main() {
  String payType = "";
  if (payType.equal("wechat")) {
    shopping(new Wepay()) // 微信支付
  }
  // else if {} ...
}
```
这样一看，增加新的支付类还是需要加 if-else ，还多了维护了一堆接口和类...

### 策略模式为的是实现模块一体化，而不是整个程序

这里涉及到了模块抽象的问题。shopping 作为一个完整的能力模块，通过策略模式实现了功能抽象，后续的修改对于 shopping 这个模块能力来说是零入侵的，透明的。

那么对于 shopping 模块的可读性、可维护、可扩展都实现了非常好的效果。而 main 就变成了一个单纯的流程，并不能抽象为一个能力主体，不存有抽象的空间。

后续对于整体程序的维护，就可以细分到每个能力模块去维护，这就是模块化的思维。

# 对代码抽象的能力 对 实现模块化十分重要
