# `fs` 模块

```javascript
// fs模块可以实现与硬盘的交互，例如文件的创建、删除、重命名、移动，
// 还有文件内容的写入、读取，以及文件夹的相关操作

// 导入 fs 模块
const fs = require("fs");
// 定义文件路径和写入字符
// 需要注意这里的相对路径可能会出现的问题：会受到命令行中的当前目录的影响，因此我们在定义路径是会大量使用到 __dirname
// __dirname：当前文件的绝对路径
const file = __dirname + "test.txt";
// 错误处理函数
const errHandler = (err) => console.log(err ? "Failed" : "Success");

// 通过 fs 新建文件
function createFileByFS() {
    const str = "new file content";
    // 1、异步创建文件
    fs.writeFile(file, str, (err) => errHandler(err));
    // 2、同步创建文件
    fs.writeFileSync(file, str);
}

// 通过 fs 追加内容
function appendContentToFileByFS() {
    const str = "\r\nnew content added";
    // 1、通过 fs.appendFile 或 fs.appendFileSync 函数追加内容到文件
    fs.appendFile(file, str, (err) => errHandler(err));
    // 2、通过对 fs.writeFile 函数传入 options 参数实现追加内容到文件
    fs.writeFile(file, str, { flag: "a" }, (err) => errHandler(err));
    // 3、创建写入流对象
    const ws = fs.createWriteStream(file);
    ws.write("abcdefg\r\n");
    ws.write("hijklmn\r\n");
    ws.write("opqrstu\r\n");
    ws.write("vwxyz\r\n");
    ws.close(); // 可选
}

// 通过 fs 读取文件
function readFileContentByFS() {
    // 1、异步读取文件内容
    fs.readFile(file, (err, data) => console.log("rf", err ? "failed" : data.toString()));
    // 2、同步读取文件内容
    const RFSBuffer = fs.readFileSync(file).toString();
    // 3、创建读取流对象
    const rs = fs.createReadStream(file);
    rs.on("data", (chunk) => {
        // 通过绑定data事件获取数据块，即chunk（每个chunk最大为64KB）
        console.log(chunk.toString());
    });
    rs.on("end", () => console.log("rs end")); // 绑定end事件，回调函数会在读取完毕后被触发
}

// fs 文件复制练习
// 1、使用 fs.readFile + fs.writeFile 实现文件复制
function __copyFile() {
    if (!fs.existsSync(file)) return;
    const fileContent = fs.readFileSync(file);
    const newFilePath = "./test-copy.txt";
    fs.writeFileSync(newFilePath, fileContent);
}
// 2、使用 fs.createReadStream + fs.createWriteStream 流式读取实现文件复制（优选，此方法占用内存小）
function __copyFileByUsingReadStream() {
    if (!fs.existsSync(file)) return;
    const newFilePath = "./test-copy.txt";
    const rs = fs.createReadStream(file);
    const ws = fs.createWriteStream(newFilePath);
    // rs.on("data", (chunk) => ws.write(chunk));
    rs.pipe(ws); // 通过管道将rs中的内容传入ws，等价于上一行代码
}

// 通过 fs 重命名文件以及移动文件
function renameFileByFS() {
    fs.rename("./test-copy.txt", "./test2.txt", (err) => errHandler(err));
}

// 通过 fs 删除文件
function deleteFileByFS() {
    // 1、通过 fs.unlink 方法删除文件
    // fs.unlink("./test2.txt", (err) => errHandler(err));
    // 2、通过 fs.rm 方法删除文件（node version 14.4+）
    fs.rm("./test2.txt", (err) => errHandler(err));
}

// 通过 fs 创建文件夹
function createDirectoryByFS() {
    // 1、创建单个文件夹
    fs.mkdir("./test", (err) => errHandler(err));
    // 2、创建多个且递归创建
    fs.mkdir("./test/a/b", { recursive: true }, (err) => errHandler(err));
}

// 通过 fs 读取文件夹内容
function listDirectoryByFS() {
    fs.readdir("./test", (err, data) => console.log(err ? "failed" : data));
}

// 删除文件夹
function remoteDirectoryByFS() {
    // fs.rm("./test/a", (err) => errHandler(err));
    fs.rmSync("./test/a", { recursive: true }, (err) => errHandler(err));
    listDirectoryByFS();
}

// 查看文件状态（属性）
function getFileStatusByFS() {
    fs.stat("./test.txt", (err, data) => {
        if (err) console.log(err);
        else {
            console.log(data);
            console.log("isFile:", data.isFile());
            console.log("isDirectory:", data.isDirectory());
        }
    });
}

// fs 文件批量重命名练习
function __renameFiles() {
    const dirPath = __dirname + "/test";
    // 1、创建文件夹
    fs.mkdir(dirPath, (err) => {
        if (err) console.log(err);
        else {
            // 2、批量创建文件
            for (let i = 1; i < 10; i++) fs.writeFileSync(`${dirPath}/${i}.txt`, "abc");
            // 3、批量重命名文件
            for (let i = 1; i < 10; i++) fs.renameSync(`${dirPath}/${i}.txt`, `${dirPath}/0${i}.txt`);
        }
    });
}

__renameFiles();
```

# `path` 模块

```javascript
const fs = require("fs");
const path = require("path");

// path.resolve
console.log(path.resolve(__dirname, "./test.txt"));

// path.sep
console.log(path.sep);

// path.parse
console.log(path.parse(__filename));

// path.basename
console.log(path.basename(__filename));

// path.dirname
console.log(path.dirname(__filename));

// path.extname
console.log(path.extname(__filename));
```