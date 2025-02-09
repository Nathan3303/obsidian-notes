
```jsx
/**
 * 1、引入 mongoose
 */
const mongoose = require("mongoose");

/**
 * 2、设置连接地址
 * @prototype mongoose.connect(uri, options);
 * @document http://mongoosejs.net/docs/connections.html
 */
mongoose.connect("mongodb://127.0.0.1/trapdesign");

/**
 * 3、连接数据库
 * @caption once 和 on 的区别以及使用
 * - 在绑定 "open" 的回调函数时推荐使用 once 绑定，因为该回调函数内部会包含业务逻辑代码，避免重复执行
 * - "error" 和 "close" 的回调函数则使用 on 绑定
 */
mongoose.connection
    // 设置连接 成功 的回调函数
    .once("open", () => console.log("Open Successfully"))
    // 设置连接 失败 的回调函数
    .on("error", () => console.log("Open Error"))
    // 设置连接 关闭 的回调函数
    .on("close", () => console.log("Connection Closed"));

// 设置延时关闭连接
setTimeout(() => mongoose.disconnect(), 6000);
```