# Promise 的理解和使用

## Promise 简述

Promise 是一个在 ES6 中提出的新规范，是 JavaScript 在进行异步编程时的新解决方案。从语法上来说，Promise 是一个构造函数；从功能上来说，Promise 对象用来封装一个异步操作并可以获取其成功/失败的结果值。

## 几个常见异步操作的使用点

1. fs 文件操作
2. 数据库操作
3. AJAX
4. 定时器

## 为什么使用 Promise

通过使用 Promise，我们可以以一个更加灵活的方式指定和调用回调函数。在还没有 Promise 之前，调用回调函数时必须在启动异步任务前指定，而 Promise 能够在启动异步任务后，给返回的 Promise 对象绑定回调函数（甚至可以在异步任务结束后指定一个或多个回调函数）。

Promise 支持链式调用，解决回调地狱问题。回调地狱是指回调函数的多层嵌套调用，外部回调函数异步执行的结果是嵌套回调函数执行的条件。回调地狱的缺点是一不便于阅读；二不便于异常处理。那么 Promise 的链式调用可以很好的解决这个问题。

回调地狱代码示例：

```jsx
asyncFunction1(options, (args) => {
    asyncFunction2(options, (args) => {
        asyncFunction3(options, (args) => {
            asyncFunction4(options, (args) => {
                // 代码不断嵌套并向前缩进
                // 不利于阅读以及维护
            });
        });
    });
});

```

## 关于 Promise 中状态的机制

Promise 的状态表现为实例对象中的一个属性 PromiseState。

PromiseState 的值有三种，分别为 pending（未决定的）、resolved/fullfilled（成功）以及 rejected（失败）。

Promise 的状态改变只能是两种情况，就是由 pending 分别变为 resolved(fullfilled) 或 rejected 这两种情况。不允许有成功变为失败或失败变为成功。并且一个 Promise 对象的状态机只能改变一次，也就是要么成功；要么失败。

无论 Promise 对象的状态机为成功或是失败，都会产生一个结果数据。成功的结果一般称为 value；而失败的结果一般称为 reason。

## 关于 Promise 中结果值的机制

Promise 的结果值表现为实例对象中的一个属性 PromiseResult。PromiseResult 中保存这成功或失败的结果值，成功或失败的记录方式一致，通过 PromiseState 区分。

PromiseResult 的值只能够通过 resolve、reject 方法进行修改。

<aside>
💡 需要注意的是当在异步任务中出现抛出错误语句时，返回的 Promise 对象中的 PromiseState 将被设置为 rejected，且 PromiseResult 的值为语句所指定错误信息。

</aside>

## Promise 的工作流程

MDN - Promise 工作流程图：

![[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png)

[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png)