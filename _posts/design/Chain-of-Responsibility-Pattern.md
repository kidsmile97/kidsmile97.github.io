---
---

# 责任链模式（Chain of Responsibility Pattern）【行为型】

责任链模式的核心是：对一个内容，做链式处理，而这个链是动态的。

> 当对一个内容可能存在多种处理方式、方式之间或存在前后关系、处理方式还可能组合使用时，应当考虑使用责任链模式

### 异步请求的链式处理

```JavaScript
// 假设接口正确的固定返回结构是 {code: 服务端接口响应代码, data: 真实数据对象}
const response = await fetch('http://xxx');
// 判断请求网络错误，相应提示、处理
if (response.status === 200) {
  return response.json();
}
// 判断接口服务错误，相应提示、处理
if (res.code === 'ok') {
  return res.data
}
```

责任链模式改造，改造后，增加新的处理器仅需补充处理器函数、补充处理器工厂，使用时按顺序、添加所需处理器即可

```JavaScript
function resProcessor(response) {
  if (response.status === 200) {
    return response.json();
  }
  // 请求网络错误提示、处理
}
function dataProcessor(res) {
  if (res.code === 'ok') {
    return res.data
  }
  // 接口服务错误提示、处理
}
const porcessorMap = {
  resProcessor,
  dataProcessor,
  // ...
}
const porcessor = (processorList) => {
  const process = processorList.map(item => porcessorMap[item]);
  return (response) => {
    let res = response;
    for (let p of process) {
      res = p(res)
    }
    return res;
  }
}
const data = porcessor(['resProcessor', 'dataProcessor'])(await fetch('xxx'));
```
