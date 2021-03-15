# [Promises/A+](https://promisesaplus.com/)

**一个由（为）诸多实现制定的优秀、可互操作的 JavaScript promises 开放标准**

一个 promise 代表一个异步操作的最终结果。和一个 promise 互操作的主要方式是通过它的 `then` 方法，这个方法注册一个回调函数来接收一个 promise 的最终值或不能兑现（fulfilled） promise 的原因。

这个规范详述了 `then` 方法的行为，提供了一个可互操作基础，所有符合 Promises/A+ 的 promise 实现都能依此提供可互操作性。因此，这个规范被认为是非常稳定的。虽然 Promises/A+ 组织可能会偶尔为了处理一些边角情况而以较小、向后兼容的更改来修订这个规范，但我们只在经过仔细的考虑、讨论和测试后才会整合较大的或不向后兼容的更改。

从历史角度看，Promises/A+ 阐明了早期 [Promises/A](http://wiki.commonjs.org/wiki/Promises/A) 提议中的行为条款，将其扩展为涵盖了实际中存在的行为并省略了规范不足或有问题的部分。

最后， Promises/A+ 核心规范不处理关于如何创建、兑现或拒绝 promises 的问题，而是选择聚焦于提供一个可互操作的 `then` 方法。未来伴随着规范的工作可能会涉及这些话题。

## 1. 术语

1.1. “promise”是一个对象或函数，拥有一个行为符合这个规范的 `then` 方法。

1.2. “thenable”是个定义了一个 `then` 方法的对象或函数。

1.3. “value”是任何合法的 JavaScript 值（包括 `undefined`、thenable 或 promise）。

1.4. “exception（异常）”是一个用 `throw` 语句抛出的值。

1.5. “reason”是一个指明了拒绝 promise 原因的值。

## 2. 要求

### 2.1. promise 状态

一个 promise 必须是三个状态之一：pending，fulfilled 或 rejected。

2.1.1. promise 处于 pending 时：

- 2.1.1.1. 可能会转换为 fulfilled 或 rejected 状态。

2.1.2. promise 处于 fulfilled 时：

- 2.1.2.1. 不可以转换为其它任何状态。

- 2.1.2.2. 必须有一个不可被改变的值。

2.1.3. promise 处于 rejected 时：

- 2.1.3.1. 不可以转换为其它任何状态。

- 2.1.3.2. 必须有一个不可被改变的原因。

这里的“不可被改变”是指不可变标识（也就是 `===`），但并不意味着深层的不可变性。

### 2.2. `then` 方法

promise 必须提供一个 `then` 方法来访问它当前或最终的值或原因。

promise 的 `then` 方法接收两个参数：

```js
promise.then(onFulfilled, onRejected)
```

2.2.1. `onFulfilled` 和 `onRejected` 都是可选参数：

- 2.2.1.1. 若 `onFulfilled` 不是函数，则必须被忽略。

- 2.2.1.2. 若 `onRejected` 不是函数，则必须被忽略。

2.2.2. 若 `onFulfilled` 是函数：

- 2.2.2.1. 必须在兑现 `promise` 后以 `promise` 的值作为第一个参数被调用。

- 2.2.2.2. 不可以在兑现 `promise` 之前被调用。

- 2.2.2.3. 只能被调用一次。

2.2.3. 若 `onRejected` 是函数：

- 2.2.3.1. 必须在拒绝 `promise` 后以 `promise` 的拒绝原因作为第一个参数被调用。

- 2.2.3.2. 不可以在拒绝 `promise` 之前被调用。

- 2.2.3.3. 只能被调用一次。

2.2.4. `onFulfilled` 或 `onRejected` 在[执行上下文](https://es5.github.io/#x10.3)栈只包含平台代码之前不可以被调用。[3.1]。

2.2.5. `onFulfilled` 或 `onRejected` 必须作为函数调用（也就是没有 `this` 值）。[3.2]。

2.2.6. `then` 可能在同一个 promise 中被多次调用。

- 2.2.6.1. 若 / 当 `promise` 被兑现时，所有各自的 `onFulfilled` 回调必须以它们来自 `then` 调用的顺序执行。

- 2.2.6.2. 若 / 当 `promise` 被拒绝时，所有各自的 `onRejected` 回调必须以它们来自 `then` 调用的顺序执行。

2.2.7. `then` 必须返回一个 promise [3.3]。

```js
promise2 = promise1.then(onFulfilled, onRejected);
```

- 2.2.7.1. 若 `onFulfilled` 或 `onRejected` 返回一个值 `x`，则运行 promise 解析程序 `[[Resolve]](promise2, x)`。

- 2.2.7.2. 若 `onFulfilled` 或 `onRejected` 抛出一个异常 `e`，则必须以 `e` 为原因拒绝 `promise2`。

- 2.2.7.3. 若 `onFulfilled` 不是函数且 `promise1` 被兑现，则必须以相同于 `promise1` 的值兑现 `promise2`。

- 2.2.7.4. 若 `onRejected` 不是函数且 `promise1` 被拒绝，则必须以相同于 `promise1` 的原因拒绝 `promise2`。
