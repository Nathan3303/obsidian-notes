# 通过 axios 函数发送

### 发送 GET 请求

当使用 axios 函数发送 HTTP 请求时，在仅指定了 URL 参数的时候默认发送 GET 请求。

```jsx
axios('/user/12345');
```

<aside>
📌 axios 的返回结果是一个 Promise 对象，因此通过 then 方法可以对返回的结果做进一步处理。

</aside>

```jsx
let res = axios({
    method: "GET",                           // 指定 HTTP请求类型
    url: "http://localhost:3000/posts/1",    // 指定 请求URL
});

res.then(    // 使用 then 方法对返回的结果做进一步处理
    (value) => console.log(value.data),
    (reason) => consoe.log(reason)
);
```

### 发送 POST 请求

```jsx
let postRes = axios({
    method: "POST",                        // 指定 HTTP请求类型
    url: "http://localhost:3000/posts",    // 指定 请求URL
    data: {                                // 指定 请求体
        title: "this is a test",
        author: "nathan",
    },
});

postRes.then(    // 使用 then 方法对返回的结果做进一步处理
		(responseRes) => console.log(responseRes)
);
```

<aside>
💡 PUT 请求和 DELETE 请求写法与上述两种请求的写法类似。

</aside>

# axios 内置的 HTTP 请求方法

除了使用 axios 函数来发送 HTTP 请求，还可以使用一些内置的方法来发送，便于直接指定要发送的 HTTP 请求类型。方法如下：

```jsx
// Request method aliases:
// For convenience, aliases have been provided for all common request methods.
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```

### 通过内置方法发送 DELETE 请求

```jsx
axios.delete(
    "http://localhost:3000/posts/1"
).then(
    (responseRes) => console.log("Delete Successful")
);
```