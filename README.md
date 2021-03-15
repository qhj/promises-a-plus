# promises-a-plus

[Promises/A+](https://promisesaplus.com/)

**一个由（为）诸多实现制定的优秀、可互操作的 JavaScript promises 开放标准**

一个 promise 代表一个异步操作的最终结果。和一个 promise 互操作的主要方式是通过它的 `then` 方法，这个方法注册一个回调函数来接收一个 promise 的最终值或不能兑现 promise 的原因。

这个规范详述了 `then` 方法的行为，提供了一个可互操作基础，所有符合 Promises/A+ 的 promise 实现都能依此提供可互操作性。因此，这个规范被认为是非常稳定的。虽然 Promises/A+ 组织可能会偶尔为了处理一些边角情况而以较小、向后兼容的更改来修订这个规范，但我们只在经过仔细的考虑、讨论和测试后才会整合较大的或不向后兼容的更改。

从历史角度看，Promises/A+ 阐明了早期 [Promises/A](http://wiki.commonjs.org/wiki/Promises/A) 提议中的行为条款，将其扩展为涵盖了实际中存在的行为并省略了规范不足或有问题的部分。

最后， Promises/A+ 核心规范不处理关于如何创建、兑现或拒绝 promises 的问题，而是选择聚焦于提供一个可互操作的 `then` 方法。未来伴随着规范的工作可能会涉及这些话题。
