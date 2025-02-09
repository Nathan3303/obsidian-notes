axios 是一个构造函数，直接调用可以默认发送一个 Get 请求。

**`axios(url[, config])`**
```js
// default method
axios('/user/12345');
```

同时，发送请求还可以通过给 axios 函数传入一个配置对象。

**`axios(config)`**
```js
axios({
	method: "post", // method: "(get|delete|patch|put|...)"
	url: "/user/12345",
	data: {
		naem: "zhangsan",
		age: 18,
	}
})

// 或

axios({
	method: "get",
	url: "https://www.baidu.com",
})
```

---

## 请求的别名函数

为了在书写上的便捷性，axios 中提供了所有常见请求的别名函数。

 - **`axios.request(config)`**
 - **`axios.get(url[, config])`**
 - **`axios.delete(url[, config])`**
 - **`axios.head(url[, config])`**
 - **`axios.options(url[, config])`**
 - **`axios.post(url[, data[, config]])`**
 - **`axios.put(url[, data[, config]])`**
 - **`axios.patch(url[, data[, config]])`**

在使用这些别名函数时，`url`，`method` 和 `data` 属性不需要在 config 中被指定。

---

## 多请求并发

在旧版本的 Axios 中，提供了用于处理并发请求的 Helper 函数，如：
**`axios.all(iterable)`** 和 **`axios.spread(callback)`** 。但在新版本中，这些函数已被弃用。对应地，使用 Promise 中的 all 方法完成多个请求并发以及处理。

---

## 创建一个 Axios 实例

通过创建一个 Axios 实例可以用于保存或者固定一个带有自定义 config 的 axios 对象。

**`axios.create([config])`**
```js
const axiosInstance = axios.create({
	baseURL: 'https://some-domain.com/api/',
	timeout: 1000,
	headers: {'X-Custom-Header': 'foobar'}
});
```

### 实例中的方法

实例中的可用方法如下所示。需要注意的是：在使用这些方法是所传入的 config 对象会被合并到实例的 config，也就是实例的 config 会受到影响。

 - **`axios#request(config)`**
 - **`axios#get(url[, config])`**
 - **`axios#delete(url[, config])`**
 - **`axios#head(url[, config])`**
 - **`axios#options(url[, config])`**
 - **`axios#post(url[, data[, config]])`**
 - **`axios#put(url[, data[, config]])`**
 - **`axios#patch(url[, data[, config]])`**
 - **`axios#getUri([config])`**

