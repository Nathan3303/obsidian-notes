
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Promise在AJAX中的实践</title>
    </head>
    <body>
        <!-- 获取接口数据：https://jsonplaceholder.typicode.com/posts -->
        <button id="btn">请求数据</button>
    </body>
    <script>
        // 获取元素
        const btn = document.querySelector("#btn");
        // 绑定事件
        btn.addEventListener("click", function (e) {
            // Promise
            const prms = new Promise((resolve, reject) => {
                // 创建 XHR 对象
                const xhr = new XMLHttpRequest();
                // 初始化
                xhr.open("GET", "https://jsonplaceholder.typicode.com/posts");
                // 发送
                xhr.send();
                // 结果响应
                xhr.onreadystatechange = function () {
                    if (xhr.readyState == 4) {
                        if (xhr.status >= 200 && xhr.status < 300) {
                            resolve(xhr.response);
                        } else {
                            reject(xhr.status);
                        }
                    }
                };
            });
            // 对结果进一步处理
            prms.then(
                (result) => console.log(result),
                (reason) => console.warn(reason)
            );
        });
    </script>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Promise-AJAX封装练习</title>
    </head>
    <body>
        <button id="btn">通过 sendAJAX 发送 GET 请求</button>
    </body>
    <script>
        /**
         * 封装函数 sendAJAX (GET)
         * @brief 该函数用于发送一个 AJAX——GET 请求
         * @param { String } url 所要请求的资源 url
         * @return Promise 对象
         */
        function sendAJAX(url) {
            // Promise
            return new Promise((resolve, reject) => {
                // 创建 XHR 对象
                const xhr = new XMLHttpRequest();
                xhr.responseType = "json";
                // 初始化 XHR
                xhr.open("GET", url);
                // 发送请求
                xhr.send();
                // 响应结果
                xhr.onreadystatechange = function () {
                    if (xhr.readyState == 4) {
                        if (xhr.status >= 200 && xhr.status < 300) {
                            resolve(xhr.response);
                        } else {
                            reject(xhr.status);
                        }
                    }
                };
            });
        }

        // 调用函数
        const btn = document.querySelector("#btn");
        btn.addEventListener("click", function (e) {
            sendAJAX("https://jsonplaceholder.typicode.com/posts").then(
                (response) => console.log(response),
                (error) => console.warn(error)
            );
        });
    </script>
</html>
```