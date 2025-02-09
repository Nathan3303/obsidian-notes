# 模块实例

```javascript
// 导入 http 模块
const http = require("http");

// 创建服务对象
const server = http.createServer((request, response) => {
    // 处理请求URL
    // let url = url.parse(request.url, true);
    let url = new URL(request.url, "http://localhost:8080");
    // console.log(url.searchParams.get("keyword"));

    // 打印请求信息
    console.log(
        request.method, // 获取请求方法
        request.headers.host, // 获取请求主机
        request.httpVersion, // 获取请求的协议版本号
        url.pathname,
        url.searchParams // 获取请求URL
        // request.headers // 获取请求头
    );

    // 设置响应状态码以及状态描述
    // response.statusCode = 404;
    // response.statusMessage = "Loveyou";

    // 设置响应头
    response.setHeader("Content-type", "text/html; charset=utf-8");
    response.setHeader("Server", "NodeJS");
    response.setHeader("MyHeader", "Nathan");

    // 设置响应体 （write允许调用零次或多次，而end必须调用且仅能够调用一次）
    response.write("你好");
    response.write(", ");
    response.end("HTTP Server!");
});

// 监听端口，启动服务
server.listen(8080, () => console.log("Server is runingon http://localhost:8080..."));
```

# 模块练习 1

```javascript
// 模块导入
const http = require("http");
const fs = require("fs");
const path = require("path");

// 定义站点根目录
const root = path.resolve(__dirname + "/http-practice3");

// 创建服务对象
const server = http.createServer((request, response) => {
    if (request.method === "GET") {
        // 识别首页路径
        if (request.url == "/") request.url = "/index.html";
        // 定义请求资源路径
        const filePath = path.join(root, request.url);
        // 判断请求资源是否带有文件后缀
        const fileExt = path.extname(filePath).slice(1);
        if (fileExt) {
            // 请求资源
            fs.readFile(filePath, (err, data) => {
                // readFile错误处理
                if (err) {
                    response.statusCode = parseReadFileError(err);
                    data = `Cannot GET ${request.url}`;
                }
                // 根据文件后缀设置MIME类型以及字符集标识
                response.setHeader("Content-type", parseMIMEType(fileExt));
                // 返回结果
                response.end(data);
            });
        } else {
        }
    } else {
        response.statusCode = 405;
        response.end("Request method not supported.");
    }
});

// 启动服务
server.listen(8080, () => console.log("server listening on http://localhost:8080"));

// 匹配MIME类型
function parseMIMEType(fileExtension) {
    // 定义MIME类型对象
    const mimeTypes = {
        css: "text/css",
        html: "text/html",
        js: "text/javascript",
        jpeg: "image/jpeg",
        png: "image/png",
        json: "application/json",
    };
    // 根据文件后缀获取MIME类型
    let mimeType = mimeTypes[fileExtension];
    // 判断是否需要拼接字符集设置
    mimeType += fileExtension === "html" ? "; charset=utf-8" : "";
    // 返回类型字符串
    return mimeType || "application/octet-stream";
}

// 匹配readFile错误
function parseReadFileError(readFileError) {
    const errCodes = {
        ENOENT: 404,
        EPERM: 403,
    };
    return errCodes[readFileError.code] || 500;
}
```

# 模块练习 2

```javascript
// 导入http模块
const http = require("http");
// 导入fs模块（获取html文件内容）
const fs = require("fs");

// 创建server对象
const server = http.createServer((request, response) => {
    // 获取响应体
    const responseText = fs.readFileSync(__dirname + "/4.http-practice2.html").toString();
    // 返回响应体
    response.end(responseText);
});

// 启动server
server.listen(8080, () => console.log("server listening on http://localhost:8080"));
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>http-practice2</title>
        <style>
            table {
                border: 1px solid #666;

                & td {
                    width: 128px;
                    height: 40px;
                    line-height: 40px;
                    text-align: center;
                    font-weight: bold;
                    font-size: 14px;
                    color: #666;
                    user-select: none;
                    cursor: pointer;
                }

                & tr:nth-child(odd) {
                    background: #dcdcdc;
                }

                & tr:nth-child(even) {
                    background: #cfc6c6;
                }
            }

            .active {
                background: #777d5d;
            }
        </style>
    </head>
    <body>
        <table>
            <tr>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td>4</td>
                <td>5</td>
                <td>6</td>
            </tr>
            <tr>
                <td>7</td>
                <td>8</td>
                <td>9</td>
            </tr>
            <tr>
                <td>10</td>
                <td>11</td>
                <td>12</td>
            </tr>
        </table>
    </body>
    <script type="text/javascript">
        const tds = document.querySelectorAll("td");
        tds.forEach((td, idx) => {
            td.addEventListener("click", ({ target }) => target.classList.toggle("active"));
        });
    </script>
</html>

```