## 安装 Express

express 本身是一个 npm 包，因此通过 npm 安装即可。

```powershell
# 1. 生成初始的 package.json
npm init -y

# 2. 安装 Express
npm install express
```

## 使用 Express

在项目目录中创建一个 js 文件，写入以下代码

```javascript
// 1. 导入 Express
const express = require("express");

// 2. 创建 Express 实例
const app = express();

// 3. 创建路由
app.get(
    "/", // 路由响应路径
    /**
     * 路由响应的回调函数
     * @param {Object} req 请求对象
     * @param {Object} res 响应对象
     */
    function (req, res) {
        // 使用 end 方法返回一个响应体（只能使用一次）
        res.end("hello, express");
    }
);

// 4. 启动 app
app.listen(3000, function () {
    console.log("Express server is listening...");
});
```

## 启动 Express

通过 node 或 nodemon 启动 Express。

> nodemon 需要提前安装：`npm install -g nodemon`

```powershell
# node
node ./express.js

# nodemon
nodemon ./express.js
```

执行上述任一指令后，出现 `Express server is listening...` 即表示启动成功。

在浏览器中输入 `http://localhost:3000/` 访问 Express 服务。
