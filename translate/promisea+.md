# ![Promises/A+](https://promisesaplus.com/assets/logo-small.png)Promises/A+

## 前言

>为什么要再翻译一遍？
>
>因为我觉得在看英文文档的过程中，你会很努力地去理解它的内容，翻译的过程中也是一个吸收的过程、加深记忆的一个过程。

## Promises/A+


**由开发人员为开发人员制订的一个开放的标准，让 JavaScript promise 安全可靠、可相互操作。**


一个 promise 代表的是一次异步操作后的最终结果。和 promise 交互的主要方式是通过 `then` 方法，该方法上的回调接收 promise 的最终值
或者这个 promise 无法实现的原因。

这个规范详细描述了 `then` 方法的行为，并且提供了一个可相互操作的基础，基于此所有符合 Promise/A+ 规范的 promise 都可以信赖。也正因为如此，
这个规范被认为是非常稳定的。尽管 Promise/A+ 组织偶尔会对规范进行小范围的向后兼容以解决新发现的极端情况。只有经过仔细的思考、讨论和测试，我们
才会集成较大的或者向后兼容的改动。

从它的发展史来看，Promise/A+ 阐述了更早之前的 [Promises/A proposal](http://wiki.commonjs.org/wiki/Promises/A) 的行为条款，并将它
扩展以覆盖实际上的行为并且忽略不明确或者有问题的部分。

最后，核心的 Promises/A+ 规范并不关心如何创建、实现或者拒绝 promise，而是专注于提供一个可交互的 `then` 方法。未来的相关规范可能涉及这些主体。

## 1. 术语

1.1 "promise" 是拥有 then 方法的一个对象或者函数，这也符合了该规范;
1.2 "thenable" 是一个定义  `then` 方法的对象或函数;
1.3 "value" 是任何合法的 JavaScript 值(包括 `undefined`, 一个 thenable,或一个 promise);
1.4 "exception" 是一个使用 `throw` 语气抛出的值;
1.5 "reason" 是能够表promise 被拒绝的原因;

## 2. 要求

### 2.1 Promise 状态

一个 Promise 必须处于这三种状态之一：pending、fulfilled, or rejected.

2.1.1 当处于 pending 时，promise :
  2.1.1.1 可能切换到 fulfilled 或者 rejected 状态
2.1.2 当处于 fulfilled 时，promise :
  2.1.2.1 不能切换成其他任何状态
  2.1.2.2 必须有一个不变的值
2.1.3 当处于 rejected 时，promise :
  2.1.3.1 不能切换成其他任何状态
  2.1.3.2 必须有一个不变的原因

这里，不能改变意味着不可更改(如 ===)，但是并不意味着永远不变。

### 2.2 `then` 方法

promise 必须提供 `then` 方法来访问它目前或者最终的值或原因。

promise 中的 `then` 方法接受两个参数：

```javascript
promise.then(onFulfilled, onRejected)
```

2.2.1 `onFulfilled` 和 `onRejected` 都是可选参数:
  2.2.1.1 如果 `onFulfilled` 不是函数，那么它会被忽略
  2.2.1.2 如果 `onRejected` 不是函数，那么它会被忽略

2.2.2 如果 `onFulfilled` 是一个函数:
  2.2.2.1 它一定会在 `promise` 完成后调用，并且将 `promise` 的值作为它的第一个参数
  2.2.2.2 它在 `promise` 完成前不能被调用
  2.2.2.3 它之多只能调用一次
2.2.3 如果 `onRejected` 是一个函数:
