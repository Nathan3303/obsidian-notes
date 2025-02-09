
```jsx
const mongoose = require("mongoose");

/**
 * 1、编译 Model
 * @brief Models 是从 Schema 编译来的构造函数。
 * - 它们的实例就代表着可以从数据库保存和读取的 documents。
 * - 从数据库创建和读取 document 的所有操作都是通过 model 进行的。
 * @caption 在 mongoose.model API 中
 * - 第一个参数是跟 model 对应的集合（collection）名字的单数形式。
 * - Mongoose 会自动找到名称是 model 名字复数形式的 collection。
 * - 比如 author 这个 model 就对应了 MongoDB 中 authors 这个 collection。
 * @caption model 名称需要大写，如 Author
 */
var schema = new mongoose.Schema({ name: "string", age: "number" });
var Author = mongoose.model("author", schema);

/**
 * 2、构造 documents，然后通过 save API 写入记录
 * @brief 通过 new 关键字构造一个 document，即 model 的实例。
 * - 传入数据对象时，匹配到的数据会被替换，未匹配到的数据根据默认值或空进行设置。
 * - 比如在 new Author 时传入对象 {name: "nathan"}，age 会被设置为 0 或空。
 * @caption 通过 model 实例的 save API 将该 document 保存到合集中。
 * @caption save API 返回一个 Promise 对象，需要通过 Promise 规范进行操作
 */
var document = new Author({ name: "nathan" });
document.save().catch((err) => console.log(err));
/**
 * @OR
 * 2、调用 model 自身的 create API 写入记录
 * @caption create API 返回一个 Promise 对象，需要通过 Promise 规范进行操作
 */
// Author.create({ name: "nathan" }).catch((error) => console.log(error));

/**
 * Mongoose 允许操作缓存
 * @brief 我们可以在程序中先声明操作，然后在数据库连接上时执行这些操作。
 * - 即不必等待连接建立成功就可以创建和操作我么你的 Mongoose Models。
 * @caption 随着下面的 mongoose.connect 调用，数据库成功连接后，上面的动作将会被执行
 */
mongoose.connect("mongodb://127.0.0.1/trapdesign");
mongoose.connection
    .once("open", () => console.log("Open successfully"))
    .on("error", () => console.log("Connection error"))
    .on("close", () => console.log("Connection closed"));
```