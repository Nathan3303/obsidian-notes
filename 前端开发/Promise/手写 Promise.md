
```jsx
class Promise {
    PromiseState = "pending"; // 状态
    PromiseResult = null; // 结果值
    Callbacks = []; // 回调函数

    constructor(executor) {
        // 缓存 this
        const _this = this;

        // resolve 方法
        function resolve(data) {
            // 判断当前状态是否不为 pending，是则不执行下面的代码
            if (_this.PromiseState !== "pending") return;
            _this.PromiseState = "fulfilled";
            _this.PromiseResult = data;
            _this.Callbacks.forEach((item) => item.onFulfilled(data));
        }

        // reject 方法
        function reject(data) {
            // 判断当前状态是否不为 pending，是则不执行下面的代码
            if (_this.PromiseState !== "pending") return;
            _this.PromiseState = "rejected";
            _this.PromiseResult = data;
            _this.Callbacks.forEach((item) => item.onRejected(data));
        }

        // 执行器函数
        if (typeof executor === "function") {
            try {
                executor(resolve, reject);
            } catch (e) {
                reject(e);
            }
        } else {
            throw "Error: executor is not a function.";
        }
    }

    /**
     * then 方法
     * @param { Function } onFulfilled
     * @param { Function } onRejected
     * @return Promise
     */
    then(onFulfilled, onRejected) {
        // 预处理 onFulfilled 以及 onRejected
        if (typeof onFulfilled !== "function") {
            onFulfilled = (value) => value;
        }
        if (typeof onRejected !== "function") {
            onRejected = (reason) => {
                throw reason;
            };
        }
        // 缓存 this
        const _this = this;
        // 返回新的 Promise 对象
        return new Promise((resolve, reject) => {
            // 回调函数返回值处理函数
            function resultHandler(cb) {
                try {
                    const res = cb(_this.PromiseResult);
                    if (res instanceof Promise) {
                        res.then(
                            (v) => resolve(v),
                            (r) => reject(r)
                        );
                    } else {
                        resolve(res);
                    }
                } catch (e) {
                    reject(e);
                }
            }
            // 根据不同情况调用回调函数
            if (this.PromiseState === "fulfilled") {
                resultHandler(onFulfilled);
            }
            if (this.PromiseState === "rejected") {
                resultHandler(onRejected);
            }
            if (this.PromiseState === "pending") {
                this.Callbacks.push({
                    onFulfilled: () => resultHandler(onFulfilled),
                    onRejected: () => resultHandler(onRejected),
                });
            }
        });
    }

    /**
     * catch 方法
     * @param { Function } onRejected 失败回调函数
     * @returns Promise Object
     */
    catch(onRejected) {
        return this.then(undefined, onRejected);
    }

    /**
     * Promise.resolve()
     * @param { any } value 值
     * @returns Promise Object
     */
    static resolve(value) {
        return new Promise((resolve, reject) => {
            if (value instanceof Promise) {
                value.then(
                    (v) => resolve(v),
                    (r) => reject(r)
                );
            } else {
                resolve(value);
            }
        });
    }

    /**
     * Promise.reject()
     * @param { any } value 值
     * @returns (rejected) Promise Object
     */
    static reject(value) {
        return new Promise((resolve, reject) => reject(value));
    }

    /**
     * Promise.all()
     * @param { Array<Promise> } promises 由多个 Promise 对象组成的数组
     * @returns Promise Object
     */
    static all(promises) {
        return new Promise((resolve, reject) => {
            const result = promises.reduce((prev, curr) => {
                curr.then(
                    (v) => prev.push(v),
                    (r) => reject(r)
                );
                return prev;
            }, []);
            result.length === promises.length && resolve(result);
        });
    }

    /**
     * Promise.race()
     * @param { Array<Promise> } promises 由多个 Promise 对象组成的数组
     * @returns Promise Object
     */
    static race(promises) {
        return new Promise((resolve, reject) => {
            promises.forEach((item) => {
                item.then(
                    (v) => resolve(v),
                    (r) => reject(r)
                );
            });
        });
    }
}
```