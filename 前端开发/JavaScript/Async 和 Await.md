# async 函数

### 函数说明

1. async 函数的返回值是一个 Promise 对象。 
2. Promise 对象的结果由 async 函数的返回值决定。
    1. 如果 async 函数返回的是一个非失败的 Promise 对象，如字符串、数组、undefined 等，则返回的必定是一个成功的 Promise 对象，该 Promise 对象的结果就为 async 函数返回的结果。
    2. 如果 async 函数返回的是一个失败的 Promise 对象，或者是抛出了错误，则返回的必定是一个失败的 Promise 对象，该 Promise 对象的结果就为指定的结果或抛出的错误。

### 使用示例

```jsx
/* 返回字符串的 async 函数，返回的是一个成功的 Promise 对象 */
async function ofunc() {
    return "successfully";
}
const excuteRes = ofunc();
console.log(excuteRes);    // Promise { <state>: "fulfilled", <value>: "successfully" }

/* 抛出错误的 async 函数，返回的是一个失败的 Promise 对象 */
async function ofunc() {
    throw "Failed";
}
const excuteRes = ofunc();
console.log(excuteRes);    // Promise { <state>: "rejected", <reason>: "Failed" }

/* 返回一个失败的 Promise 对象的 async 函数 */
async function ofunc() {
    return new Promise((resolve, reject) => {
        reject("Failed");
    });
}
const excuteRes = ofunc();
console.log(excuteRes);
// Promise { <state>: "pending" }  <state>: "rejected"  <reason>: "Failed"

/* 返回一个失败的 Promise 对象的 async 函数 */
async function ofunc() {
    return new Promise((resolve, reject) => {
				resolve("successful");
    });
}
const excuteRes = ofunc();
console.log(excuteRes);
// Promise { <state>: "pending" }  <state>: "fulfilled"  <reason>: "successful"
```

# await 表达式

### 表达式说明

1. await 必须写在 async 函数中。
2. await 右侧的表达式一般为 Promise 对象。
3. await 返回的是 Promise 成功的值。
4. await 的 Promise 失败了，就会抛出异常，需要通过 `try … catch …` 进行捕获处理。

### 使用示例

```jsx
/* 使用 await 接收一个成功的 Promise */
// 定义一个 Promise 对象，并且是成功的 Promise
const oPrms = new Promise((resolve, reject) => {
    resolve("successful");
});
// 定义 async 函数
async function main() {
    let excuteRes = await oPrms;
    console.log("@await表达式的使用 ", excuteRes);    // @await表达式的使用  successful
}
// 调用函数
main();

/* 使用 await 接收一个成功的 Promise */
// 定义一个 Promise 对象，是一个失败的 Promise
const oPrms = new Promise((resolve, reject) => {
    reject("Load Failed");
});
// 定义 async 函数
async function main() {
		// 通过 try...catch 处理失败的 Promise 返回
		try {
        let excuteRes = await oPrms;
    } catch (e) {
        console.log("@await表达式的使用 ", e);    // @await表达式的使用  Load Failed
    }
}
// 调用函数
main();
```

# async 和 await 封装 AJAX 请求

```jsx
// 发送 AJAX 请求，返回的结果是 Promise 对象
function sendAJAX(url) {
    return new Promise((resolve, reject) => {
        const oXML = new XMLHttpRequest();
        oXML.open("GET", url);
        oXML.send();
        oXML.onreadystatechange = function () {
            if (oXML.readyState === 4) {
                if (oXML.status >= 200 && oXML.status < 300)
                    resolve(oXML.response);
                else reject(oXML.status);
            }
        };
    });
}
// async 搭配 await 发送 AJAX 请求
async function send() {
    let sendRes1 = await sendAJAX(
        "https://api.github.com/search/users?q=11"
    );
    console.log(sendRes1);
    let sendRes2 = await sendAJAX(
        "https://api.github.com/search/users?q=22"
    );
    console.log(sendRes2);
}
send();
```