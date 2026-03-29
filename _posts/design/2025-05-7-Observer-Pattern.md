---
title: 设计：观察者模式
date: 2025-05-07 19:56:43 +0800
categories: [Design Pattern]
tags: [观察者模式 js, 设计模式, design pattern, 观察者模式, Observer Pattern, strategy in js]
---

# 观察者模式（Observer Pattern）【行为型】

设计了一种一对多的依赖关系，在保持消息和协作的关系同时解耦。

> ok说人话：A 需要和 1、2、3...协同
1. 正常逻辑：先把 1、2、3...引入到 A 中，A 改变时通知 1、2、3...
2. 观察者模式：
- 创建一个 Subject 绑定 A，A 改变时让 Subject 通知而不是自己通知；
- 为 1、2、3...创建各自的 Observer，然后 subject 关联所有的 observer，让收到通知的 observer 执行而不是自己亲自执行
- A 关联了 Subject，1、2、3 关联了 Observer，实现 A 与 1、2、3...的解耦

> 发布订阅者模式【行为型】，与观察者模式理念基本相同，都是由一个目标，多个观察者组成的一对多关系设计模式

### 观察者模式的核心

拍卖会上，有一件难得的`珍品`备受关注，所有的`买家`都必须时刻紧盯，不断报价才有机会拿下。

> `珍品` = subject，它作为必须实时通知所有感兴趣的买家，它的实时价格，而且它并不在意在某个时候对它感兴趣的人多了还是少了

> `买家` = observer，作为一个对一件拍品同时感兴趣的群体，他们都关注拍品价格，但是并不关心别人对目标的看法

##### 特点 1：一对多的关系

买家有很多，但拍品只有一个，每个人都关注着目标

##### 特点 2：`多`之间相互并不关心

买家与买家之间相互并不关心，他们只关注拍品共享出来的信息，而不管其它买家的任何动作和行为。

##### 特点 3：`一`本身只对外提供它想要暴露的消息，不关心观察者利用信息的具体行为

拍品只对外实时更新它的价格，买家知道价格后或许有人放弃、有人加入、有人马上出价、有人在考虑是否出价...但这些拍品本身都不在意

### 观察者模式的几种模型

由上清楚了观察者模式的核心：subject & observer ，由此可以延伸出来的模式模型有 3 种

##### 模型一：两者模型

subject: 负责搜集所有的观察者并发布信息

observer: 负责监听到信息后安排各自的行为策略

```ts
interface Observer<T> {
  update(message: T): void;
}
interface Subject<T> {
  subscribers: Observer<T>[];
  subscribe(Observer<T>): void;
  unsubscribe(Observer<T>): void;
  notify(): void;
}
const subject = new Subject();
const obs1 = new Observer();
const obs2 = new Observer();
const obs3 = new Observer();
subject.subscribe(obs1);
subject.subscribe(obs2);
subject.unsubscribe(obs1);
subject.subscribe(obs3);
```

##### 模型二：三者模型

target: 监听目标

subject: 监听并发布消息

observer: 收到信息后安排各自的行为策略

```ts
interface Target {
  property1: string;
  property2: number;
}
interface Observer<T> {
  update(message: T): void;
}
class Subject<T> {
  subscribers: Observer<T>[];
  constructor() {
    this.subscribers = [];
  }
  listen(obj: Target) {
    // const
  };
  subscribe(obs: Observer<T>) {
    this.subscribers.push(obs);
  };
  unsubscribe(obs: Observer<T>){
    const index = this.subscribers.findIndex(item => item === obs);
    this.subscribers.splice(index, 1);
  };
  notify(message: T){
    subscribers.forEach(item => item.update(message));
  };
}

const t = new Target();
const s = new Subject();
const obs1 = new Observer();
const obs2 = new Observer();
s.listen(t);
s.subscribe(obs1);
s.subscribe(obs2);
s.notify("this is a message by update target");
```

### 观察者模式的使用场景

### 进阶版：消息处理中心

### 发布订阅模式与观察者模式的区别

由上可知观察者模式，更注重的是对目标进行监听执行

发布订阅模式，则是两者兼重，一方面需要关注目标进行监听，一方面也关注变化进行通知
