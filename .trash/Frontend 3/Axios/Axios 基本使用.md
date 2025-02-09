Axios 是一个基于 Promise 的网络请求库，作用于 node.js 和浏览器中。Axios 是 isomorphic 的，即同一套代码可以运行在 node.js 和浏览器中。Axios 在 node.js 服务端使用的是 node.js 原生的 http 模块，而在客户端中使用的是 XMLHttpRequest。

在客户端中，Axios 和 jQuery 中发送网络请求的部分方法一样也是对 XMLHttpRequest 的封装，只不过它是 Promise 的实现版本，符合最新的 ES 规范。

---

## 引入 Axios

### 通过 CDN 连接直接引入

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.27.2/axios.min.js"></script>
```

### 在构建工具中引入 Axios

在构建工具中，通过包管理工具下载并引入的方式使用 Axios。

#### 下载 axios

```shell
# npm
npm install axios

# bower
bower install axios

# yarn
yarn add axios

# pnpm
pnpm add axios
```

#### 引入 axios

```js
// 通过 require 引入
const axios = require("axios").default;

// 通过 import 引入
import axios from 'axios';
```

除了引入默认暴露的 axios 对象，还可以引入分别暴露的一些方法。

```js
// 通过 import 引入
import axios, { isCancel, AxiosError } from 'axios';

// 若使用 require ，只能引入默认暴露的 axios 对象
const axios = require('axios');
console.log(axios.isCancel('something'));

```

---

## 快速使用 Axios

### 发送 Get 请求

通过 Axios 发送一个 GET 请求，可以使用 axios 对象中的 `get` 方法。

```js
import axios from 'axios';

/**
 * 发送一个 get 请求
 * @param { String } url
 * @param { Object } options
 */

axios.get('/users?id=123')
	.then(function (response) {
		// success handler
		console.log(response);
	})
	.catch(function (error) {
		// error handler
		console.warn(error);
	})
	.finally(function () {
		// always executed
	})

// 或

axios.get('/user', { 
	params: { id: '123' } 
})
	.then(/* success handler */)
	.catch(/* error handler */)
	.finally(/* always executed */)
```

### 发送 Post 请求

通过 Axios 发送一个 POST 请求，可以使用 axios 对象中的 post 方法。

```js
import axios from 'axios';

axios.post('/user', {
	name: "zhangsan",
	age: 18
})
	.then(function (response) {
		console.log(response);
	})
	.catch(function (error) {
		console.warn(error);
	})
```

### 连续发送多次请求

通过 Axios 连续发送多次请求，可以讲每一次请求封装成一个自定义函数，比如：

```js
function getUserAccount() {
	return axios.get('/user/12345');
}

function getUserPermissions() {
	return axios.get('/user/12345/permissions');
}
```

对于请求结果，可以通过 Promise 中的 `all` 方法进行逐个处理：

```js
Promise.all([
	getUserAccount(),
	getUserPermissions()
])
	.then(function (results) {
		const acct = results[0];
		const perm = results[1];
	});
```

### 配合 async 和 await

通过定义一个带有 `async` 关键字的外层函数包裹发送请求的语句以实现一个异步的网络请求任务。

由于 `await` 右侧的表达式所返回的 Promise 是一个失败的状态，则会抛出异常，需要通过 `try ... catch ...` 语句进行错误捕获和处理。

```js
async function getUser() {
	try {
		const response = await axios.get('/user?id=12345');
		console.log(response);
	} catch (error) {
		console.error(error);
	}
}
```

