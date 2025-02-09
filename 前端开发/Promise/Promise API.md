## Promise 构造函数

`Promise(excutor) { }`

参数 executor 函数称为执行器函数。形式为：`(resolve, reject) => {}` 。其中 `resolve` 为异步任务成功时调用的函数；`reject` 为异步任务失败时调用的函数。执行器函数是**同步执行**的（不包括异步代码，如定时器、网络请求等）。

`resolve` 和 `reject` 函数都接收一个参数，该参数用于指定所在 Promise 对象的 PromiseResult 。

## then

`prms.then(onFulfillment, onRejected)`

`then` 用于指定成功或失败的回调函数，并且在该 Promise 对象的状态作出改变时执行对应的回调函数。其中 `onFulfillment` 为成功时的回调函数：`(value) => { /*...*/ }` ；`onRejected` 函数为失败时的回调函数：`(reason) => { /*...*/ }`。

## catch

`prms.catch(onRejected)`

`catch` 方法仅用于处理 Promise 对象的失败情况，指定并执行失败的回调函数 `onRejected` 函数。

## resolve

`Promise.resolve(value)`

`Promise.resolve` **属于 Promise 函数对象**。`Promise.resolve` 方法用于快速地生成一个成功或者失败的 Promise 对象。该方法接收一个参数，该参数的值将会用作新生成的 Promise 对象中 PromiseResult 的值。

```jsx
/**
 * Promise.resolve()
 * @brief
 *  - resolve 方法不属于实例对象，而是属于 Promise 函数对象。
 *  - resolve 方法用于快速生成一个成功或失败的 Promise 对象。
 *  - 该方法接受一个参数，参数值将用作 PromiseResult 的值，而 PromiseState 则会根据参数值判断得到。
 * @caption 关于传入参数对于返回结果的影响
 *  - 若传入的参数为 非Promise 类型的对象，则返回的 Promise 对象是成功的。
 *  - 若传入的参数为 Promise 类型的对象，则根据其状态决定返回的 Promise 对象的状态。
 */

let prms;

prms = Promise.resolve(123);
console.log(prms); // fullfilled - 123

prms = Promise.resolve(undefined);
console.log(prms); // fullfilled - undefined

prms = Promise.resolve("abc");
console.log(prms); // fullfilled - abc

prms = Promise.resolve(false);
console.log(prms); // fullfilled - false

prms = Promise.resolve(new Promise((resolve, reject) => reject("error")));
console.log(prms); // rejected - error
```

## reject

reject 方法和 resolve 一样不属于实例对象，属于 Promise 函数对象。reject 方法用于生成一个失败的 Promise 对象。该方法接受一个参数，参数值将用作 PromiseResult 的值。

```jsx
/**
 * Promise.reject
 * @brief
 *  - reject 方法和 resolve 一样不属于实例对象，属于 Promise 函数对象。
 *  - reject 方法用于生成一个失败的 Promise 对象。
 *  - 该方法接受一个参数，参数值将用作 PromiseResult 的值。
 */
let prms;

prms = Promise.reject(123);
console.log(prms); // rejected - 123

prms = Promise.reject("abc");
console.log(prms); // rejected - abc

prms = Promise.reject(new Promise((resolve, reject) => resolve("abc")));
console.log(prms); // rejected - Promise{ "abc" }
```

## all

all 方法一样属于 Promise 函数对象。all 方法用于检查多个 Promise。该方法接受一个参数，该参数为多个 Promise 对象组成的数组。all 方法会返回一个新的 Promise 对象，该对象的成功与失败取决于数组中的 Promise 对象。当数组中所有 Promise 对象的状态为成功，那么返回的新 Promise 对象则为成功，且 PromiseResult 为数组中所有 Promise 对象成功的结果；当数组中有一个 Promise 对象的状态为失败，那么返回的新 Promise 对象则为失败，且 PromiseResult 为改 Promise 对象失败的结果。all 方法可以简单理解为：所有 Promise 都成功了才算成功，只要有一个失败了就算失败。

```jsx
/**
 * Promise.all()
 * @brief
 *  - all 方法一样属于 Promise 函数对象。
 *  - all 方法用于检查多个 Promise。该方法接受一个参数，该参数为多个 Promise 对象组成的数组。
 *  - all 方法会返回一个新的 Promise 对象，该对象的成功与失败取决于数组中的 Promise 对象。
 *  - 当数组中所有 Promise 对象的状态为成功，那么返回的新 Promise 对象则为成功，且 PromiseResult 为数组中所有 Promise 对象成功的结果；
 *    当数组中有一个 Promise 对象的状态为失败，那么返回的新 Promise 对象则为失败，且 PromiseResult 为改 Promise 对象失败的结果。
 *  - all 方法可以简单理解为：所有 Promise 都成功了才算成功，只要有一个失败了就算失败。
 */

const p1 = new Promise((resolve, reject) => resolve("p1 ok"));
const p2 = new Promise((resolve, reject) => resolve("p2 ok"));
const p3 = new Promise((resolve, reject) => resolve("p3 ok"));
const p4 = new Promise((resolve, reject) => resolve("p4 ok"));
const p5 = new Promise((resolve, reject) => resolve("p5 ok"));
const p6 = new Promise((resolve, reject) => reject("p6 not ok"));

let prms;

prms = Promise.all([p1, p2, p3, p4, p5]);
prms.then((value) => console.log(value)); // [ 'p1 ok', 'p2 ok', 'p3 ok', 'p4 ok', 'p5 ok' ]

prms = Promise.all([p1, p2, p3, p4, p5, p6]);
prms.then(
    () => {},
    (reason) => console.log(reason) // p6 not ok
);
```

## race

race 方法属于 Promise 函数对象。race 方法用于检查多个 Promie。该方法接受一个参数，该参数为多个 Promise 对象组成的数组。race 方法会返回一个新的 Promise 对象，该对象的成功玉石白取决于数组中的 Promise 对象。race 本意为竞赛，在这里指的是谁先改变状态，返回的结果就以其为准。race 方法可以简单理解为：多个 Promise 赛跑，谁的状态最先变化，就返回谁。

```jsx
/**
 * Promise.race()
 * @brief
 * - race 方法属于 Promise 函数对象。
 * - race 方法用于检查多个 Promie。该方法接受一个参数，该参数为多个 Promise 对象组成的数组。
 * - race 方法会返回一个新的 Promise 对象，该对象的成功或失败取决于数组中的 Promise 对象。
 * - race 本意为竞赛，在这里指的是谁先改变状态，返回的结果就以其为准。
 * - race 方法可以简单理解为：多个 Promise 赛跑，谁的状态最先变化，就返回谁。
 */

const p1 = new Promise((resolve, reject) => {
    setTimeout(function () {
        resolve("p1 ok");
    }, 1000);
});
const p2 = new Promise((resolve, reject) => {
    setTimeout(function () {
        resolve("p2 ok");
    }, 500);
});
const p3 = new Promise((resolve, reject) => {
    setTimeout(function () {
        resolve("p3 ok");
    }, 300);
});

const p4 = new Promise((resolve, reject) => {
    setTimeout(function () {
        reject("p4 not ok");
    }, 100);
});

let prms;

prms = Promise.race([p1, p2, p3]);
prms.then((value) => console.log(value)); // p3 ok

prms = Promise.race([p1, p2, p3, p4]);
prms.then(
    (value) => console.log(value),
    (reason) => console.log(reason) // p4 not ok
);
```