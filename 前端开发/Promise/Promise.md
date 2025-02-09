# Promise 使用示例

```jsx
const p = new Promise((resolve, reject) => {
    let isSuccess = true;
    if (isSuccess) resolve(isSuccess);
    reject(isSuccess);
});

p.then(
    resovled = isSuccess => console.log(isSuccess),
    rejected = isSuccess => console.log(isSuccess),
)
```

# Promise 中的 then 方法

在 Promise 中，方法 then 用于执行异步操作成功或失败时的回调。

调用 then 方法，then 方法返回的结果是一个 Promise 对象，对象状态由回调函数的执行结果决定。

如果回调函数中返回的结果是非 Promise 类型的值，则此时 then 返回的 Promise 对象中的状态 PromiseStatus 默认为成功，且该对象中的 PromiseValue 的值为回调函数返回值。

```jsx
// 创建 Promise 实例
const p = new Promise((resolve, reject) => {
    let isSuccess = true;
    if (isSuccess) resolve(isSuccess);
    reject(isSuccess);
});

// 通过 then 方法执行回调函数
const res = p.then(
    (value) => "resolved!",
    (reason) => "rejected!"
);

console.log(res);    // Promise { <state>: "fulfilled", <value>: "resolved!" }
```

由于 then 方法可以返回一个 Promise 对象，因此该 Promise 对象也可以调用其原型上的 then 方法，因此就形成了 then 的链式调用，即多个异步任务相继处理。

```jsx
const res1 = p.then(
    (value) => "res1 was resolved!"
);

const res2 = res1.then(
    (value) => "res2 was resolved!"
);

console.log(res1, res2);
// res1 == Promise { <state>: "fulfilled", <value>: "res1 was resolved!" }
// res2 == Promise { <state>: "fulfilled", <value>: "res2 was resolved!" }
```

# Promise 中的 catch 方法

catch 也可以用于执行回调函数，当异步操作执行出现错误，即只有是一个带有失败状态的 Promise 实例才适合通过 catch 方法执行回调。

catch 方法需要一个参数，也就是处理失败的回调函数。就像 then 方法忽略第一个参数的形式，因此 catch 是 then 处理失败操作的一个语法糖。

```jsx
const p = new Promise((resolve, reject)=>{
		reject("Failed!");
});

p.catch(reason => {
		console.warn(reason);    // Failed!
});
```