## 基本响应设置

Express 框架封装了部分原生的 API 以便于向客户端响应数据。Express 兼容所有原生 HTTP 模块的响应方式。

这里我们通过两个例子分别展示原生 HTTP 模块以及 Express 框架所封装的两种响应处理方式。

原生 HTTP 模块响应：

```javascript
app.get("/api", (req, res) => {
    // 设置响应状态码
    res.statusCode = 500;
    // 设置响应消息
    res.statusMessage = "Not ok";
    // 设置响应头
    res.setHeader("xxx", "yyy");
    // 设置响应体
    res.write("Hello, ");
    // 设置响应体并返回数据
    res.end("Express");
});
```

Express 响应：

```javascript
app.get("/api", (req, res) => {
    // 设置响应状态码
    // res.status(500);
    // 设置响应头
    // res.set('xxx', 'yyy');
    // 设置响应体并返回数据
    // res.send("Hello, Express");

    // 连续执行
    res.status(500).set("xxx", "yyy").send("Hello, Express");
});
```

## 其他响应设置

除了基本的状态码、请求头、文字响应设置外，我们还能够设置重定向、传输文件、设置 JSON 响应以及返回一个文件（如 html）：

```javascript
// 重定向
app.get("/other", (req, res) => {
    res.redirect("https://www.baidu.com");
});

// 下载响应
app.get("/download", (req, res) => {
    res.download(__dirname + "/package.json");
});

// JSON 响应
app.get("/json", (req, res) => {
    res.json({ a: 1, b: 2 });
});

// 文件响应
app.get("/file", (req, res) => {
    res.sendFile(__dirname + "/1.quick-start.js");
});
```

## 获取 POST 请求体数据

客户端发送过来的 POST 请求往往携带参数数据，比如认证信息、请求的参数值等。那么此时我们需要获取请求体中的这些数据来判断返回的信息。

在 Express 中，可以使用 body-parser 这个 npm 包来快速获取请求体中的数据。

### 安装 body-parser

body-parser 是作为 Express 的中间件所使用。

执行命令：`npm install body-parser` 安装 body-parser。

### 使用 body-parser

```javascript
// 引入 Express
const express = require("express");
// 引入 body-parser
const bodyParser = require("body-parser");
// 创建 Express 实例
const app = express();

// 解析 JSON 格式的请求体的中间件
// const jsonParser = bodyParser.json();
// 解析 querystring 格式请求体的中间件
const urlencodeParser = bodyParser.urlencoded({ extended: true });

// 设置 GET 路由规则
app.get("/login", (req, res) => {
    // 返回表单 HTML 页面
    res.sendFile(__dirname + "/index.html");
});

// 设置 POST 路由规则
app.post("/login", urlencodeParser, (req, res) => {
    // 中间件函数执行后，req中会被添加一个body属性，其中就为post请求的请求体
    console.log(req.body);
    res.send(req.body);
});

// 兜底路由
app.all("*", (req, res) => res.end("Page not found"));

// 启动app
app.listen(3000, () => console.log("Server is running ..."));
```

## 防盗链

防盗链，顾名思义就是非本站点的程序访问站点内资源。具体就是判断请求的来源地址，若发现请求的来源地址不是本站点下的域名或 IP 地址，那么就不会返回指定资源给请求方。

防盗链的实现方式：通过判断请求头中所携带的 referer 属性来决定是否返回资源。

下面是防盗链的一个简单实现：

```javascript
// 引入 Express
const express = require("express");
// 创建 Express 实例
const app = express();

// 实现防盗链中间件
const checkReferer = (req, res, next) => {
    // 检测请求头中的 referer 标识
    const referer = req.get("referer");
    // 判断 referer 是否存在
    if (referer) {
        // 获取 referer 中的主机名
        const { hostname } = new URL(referer);
        // 判断主机名是否满足防盗链要求
        if (hostname !== "127.0.0.1") {
            res.status(403).send("403 Forbidden");
            return;
        }
    }
    next();
};

// 全局应用防盗链中间件
app.use(checkReferer);

// 全局应用静态资源中间件
app.use(express.static(__dirname + "/public"));

// 启动服务
app.listen(3000, () => console.log("Server is running ..."));
```

