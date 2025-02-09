
```jsx
// 引入 fs
const fs = require("fs");

// Promise
const prms = new Promise((resolve, reject) => {
    fs.readFile(__dirname + "/README.md", (err, data) => {
        if (err) reject(err);
        resolve(data);
    });
});
prms.then(
    (data) => console.log(data.toString()),
    (err) => console.log(err)
);
```

```jsx
// 引入 fs
const fs = require("fs");

/**
 * 封装函数 mineReadFile
 * @brief 该函数用于读取文件内容
 * @param { String } path 所要读取的文件路径
 * @return Promise 对象
 */
function mineReadFile(path) {
    // Promise
    return new Promise((resolve, reject) => {
        // 读取文件内容
        fs.readFile(path, (error, data) => {
            // 失败
            if (error) reject(error);
            // 成功
            resolve(data.toString());
        });
    });
}

// 调用函数
mineReadFile(__dirname + "/README.md").then(
    (value) => console.log(value),
    (error) => console.warn(error)
);
```