## 什么是中间件

中间件，Middleware。用来特指业务流程的中间处理环节。就像一坨面粉最终称为饼干，其中的各个步骤就称为中间的处理环节。在 Express 服务器中，也是这样的道理，当客户端请求到达 Express 服务器后，调用多个中间件来对这次请求进行预处理，处理完毕之后，才会将处理完成的结果给到路由来完成响应。

在 Express 中，中间件的本质是一个回调函数，可以与路由回调一样访问请求（request）对象和响应（response）对象。

## 中间件的作用和类型

在遇到多个路由回调中的逻辑代码部分相同时，可以通过中间件来封装这些相同的代码，重复调用，这样就可以简化我们在路由回调中的代码了。

中间件分为全局中间件和路由中间件。他们的区别是：全局中间件在任一路由匹配成功时，路由回调执行前都会执行；而路由中间件只在指定路由匹配成功时，路由回调执行前执行。

> 现实例子：全局中间件相当于高铁进站口；路由中间件相当于高铁检票口。

## 中间件的定义方式

Express 中间件的定义语法如下：

```javascript
/**
 * 中间件定义语法
 * @param req 请求对象
 * @param res 响应对象
 * @param next 处理链流转函数
 */
const myMiddleware = function (req, res, next) {
    /* do something ... */
    next();
}
```

可以看到中间件的定义方式与路由回调非常类似，仅仅只是多了一个形参 `next`。这里的 `next` 是一个函数，是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件，若后续没有中间件则转交给最终的路由回调进行处理。因此我们的 `next()` 必须写，并且需要写在逻辑分支的最后。

## 全局应用中间件

被全局应用的中间件称为全局中间件。全局中间件会在每一个请求到达后执行。

```javascript
// 导入express
const express = require("express");
// 导入 fs 和 path
const fs = require("fs");
const path = require("path");

// 创建express对象
const app = express();

// 定义中间件，记录每个请求的 url 和 IP 地址。
const recordAccessLogMiddleware = function (req, res, next) {
    // 获取 url 和 ip 地址
    const { url, ip } = req;
    // 记录 url 和 ip 地址
    fs.appendFile(path.resolve(__dirname, "./access.log"), `${ip} - ${url}\r\n`, (err) => err && console.log(err));
    // 流程继续
    next();
};

// 全局应用中间件函数
app.use(recordAccessLogMiddleware);

// 响应GET请求（设置路由）
app.get("/", (req, res) => res.end("Hello, express"));

// 启动服务
app.listen(3000, () => console.log("Server is running"));
```

从上面的代码可以看出，将一个中间件设为全局只需要将中间件作为实参传入 `app.use()` 即可。若需要定义多个，则调用多次 `app.use()` 即可。

## 局部应用中间件

应用于部分路由的中间件称为局部中间件。局部中间件只在请求满足指定路由时执行。

```javascript
// 需求：针对 /admin 和 /setting 的请求，要求 URL 携带 code=521 参数，若检测到未携带则提示 参数错误

// 定义中间件
const checkCodeMiddleware = (req, res, next) => {
    if (req.query.code == "521") 
        next();
    else 
        res.send("参数错误");
};

// 创建路由并应用中间件
app.get(
    "/admin",
    checkCodeMiddleware, // 对于该路由应用中间件
    (req, res) => res.send("后台页")
);
app.get(
    "/setting",
    checkCodeMiddleware, // 对于该路由应用中间件
    (req, res) => res.send("设置页")
);
```

从上面的代码可以看出，应用局部中间件只需要将中间件作为实参传入到对应路由的路径形参后即可。若当一个路由需要应用多个局部中间件时我们可以有如下两种写法：

```javascript
app.get('/home', mw1, mw2, mw3, (req, res) => {
    /* do something ... */
})
```

或者是通过数组的形式

```javascript
app.get('/home', [mw1, mw2, mw3], (req, res) => {
    /* do something ... */
})
```

## Express 中的静态资源中间件

### 静态资源中间件的相关概念

Express 中封装了一个静态资源中间件 exprss.static()，用于在客户端请求某个静态资源时提供。在使用该中间件前，我们需要了解一个概念：静态资源以及静态资源目录。

首先，静态资源是什么？我们一般将内容长时间不做更改的文件就成为静态资源。比如：html 文档、css 文档、图片、音视频等。

其次，静态资源目录是什么？我们习惯将静态资源分类放置于一个目录（通常为 public）中，这样通常方便读取。那么我们就称这样的一个目录为静态资源目录。

了解了上述概念后，下面就通过 Express 中的静态资源中间件实现静态资源的请求响应。

### 静态资源中间件的使用

首先项目根目录中需要创建如下目录结构：  
- public  
    - css  
        - index.css  
    - html  
        - index.html  
    - images  
        - index.png  

使用静态资源中间件：

```javascript
app.use(express.static(__dirname + "/public"));
```

只要我们使用了这个中间件，就相当于 public 目录作为了网站的根目录。

### 静态资源中间件的注意事项

public 目录下的 index.html 是默认返回的静态资源。

由于静态资源中间件是全局的，当静态资源中间件与路由规则都能处理当前请求时，往往静态资源中间件会先于路由规则作出处理，进而返回静态资源。

路由规则一般响应动态资源，如需要从数据库中调取的动态数据，以及一些逻辑处理；而静态资源中间件只做静态资源的响应。