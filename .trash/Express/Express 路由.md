## 什么是路由

路由确定了应用程序如何响应客户端对特端点的请求。

## 路由的使用

一个路由的组成有请求方法、路径以及回调函数组成。在 Express 中提供了一系列的方法，便于我们创建和使用路由。使用格式如下：

```javascript
app.<method>(path, callback);
```

## 路由使用示例

```javascript
/**
 * 通过 Express 实例中的 get 方法处理客户端发送的 GET 请求。
 * 通过回调函数中的逻辑代码处理请求并返回指定内容。
 * 当前路由匹配为 /home ，因此客户端需要向 http://localhost:3000/home 这个 URL 发送 GET 请求才能够被该路由所识别并返回正确的内容。
 */
app.get("/home", (req, res) => res.end("首页"));

app.get("/news", (req, res) => res.end("新闻页"));

// post 亦是如此
app.post("login", (req, res) => res.end("登录成功"));

/**
 * 多个路由规则根据代码书写顺序匹配，
 * 因此我们可以在最后定义一个根路由来匹配超出范围的错误请求。
 */
app.get("/", (req, res) => res.end("找不到页面 404"));
```

## 路由类型以及参数获取

路由的类型有两种，分别是字面型和通配型。字面型顾名思义，在匹配时会进行完整的字符串配对，只有在两个路由一致时才会触发路由的成功配对，执行回调函数；而通配型则通过一个占位符来获取路由信息，比如 `/:id.html` 就可以匹配任一 uri 为 `/xxx.html` 的请求，且能够在回调函数的形参 req 中获取 id 的值（在该例子中，id 的值为 xxx）。

通配型的请求参数通常被用在同一种类不同分支的路由中。比如商品页面，很有可能 uri 的都为 `/goods/xxx` ，需要根据传入的 `xxx` 来识别出请求的商品信息进而返回，因此我们可以设置我们的路由为 `/goods/:id` 以识别这一类型的路由。若这里通过字面型，那么我们需要定义等同于商品数量的路由数目，显然不合适。

```javascript
// 导入express
const express = require("express");

// 创建express对象
const app = express();

// 设置一个字面型的路由，并获取其中的参数
app.get("/home", (req, res) => {
    // 通过原生操作的方式获取请求体中的数据
    console.log(req.method);
    console.log(req.url);
    console.log(req.httpVersion);
    console.log(req.headers);

    // 通过 Express 提供的方式可以 快速的 获取请求体中的 某一个数据
    console.log(req.path); // 请求路径
    console.log(req.query); // 请求参数
    console.log(req.ip); // 请求 ip 地址
    console.log(req.get("host")); // 通过 get 方法获取请求头中的主机名

    // 返回响应体
    res.end("字面型路由");
});

// 设置一个通配型的路由，并获取其中的参数
app.get("/:id.html", function (req, res) {
    // 输出通配参数
    console.log(req.params.id);
    // 返回响应体
    res.end("通配型路由 - 单个参数的设置");
});

// 设置通配型路由的多个参数
app.get("/:view/:category", (req, res) => {
    // 输出通配参数
    console.log(req.params.view);
    console.log(req.params.category);
    // 返回响应体
    res.end("通配型路由 - 多个参数的设置");
});

// 启动服务
app.listen(8080, () => {
    console.log("Express is listening on http://localhost:8080");
});
```

## 路由模块化

在此之前，我们的所有路由规则都写在了同一个文件下，当我们的项目逐渐庞大，路由数目逐渐增多，若我们还是依然所有路由规则都写在同一文件下显然不合适。这里提出了路由模块化的概念，就是将多个同一种类不同逻辑的路由放在单独的一个文件中，通过暴露的方式在主文件中引入对应的路由规则。通过路由模块化我们能够简化服务主文件的代码量，也便于路由逻辑的维护。

通常我们的路由模块文件会放在项目目录中 routes 文件夹下，文件名称通常为 xxxRouter.js 。

### 创建路由模块

我们首先需要在项目根目录中创建文件夹 routes 。接着创建路由模块文件（这里创建 shotsRouter.js）。

```javascript
// 导入 Express
const express = require("express");

// 创建路由对象（路由对象相当于小型的app对象）
const router = express.Router();

// 创建路由规则
router.get("/shots", (req, res) => {
    res.send("作品展示页");
});
router.get("/shots/:view/:category", (req, res) => {
    res.send("作品展示页 - 包含视图和分类参数");
});

// 暴露当前router
module.exports = router;
```

### 在主文件中引入路由模块

通过 require 语句引入路由模块，通过应用全局中间件的方式应用路由模块。示例代码如下：

```javascript
// 导入模块
const express = require("express");
// 导入路由模块
const shotsRouter = require("./routes/shotsRouter");
// 创建并开启 express
const app = express();
// 应用路由模块
app.use(shotsRouter);
// 启动服务
app.listen(3000, () => console.log("Server is running ..."));
```
