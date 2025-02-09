
```jsx
const mongoose = require("mongoose");

/**
 * 字段类型规范
 * @caption 常用字段类型标注：
 *  - String 字符串
 *  - Number 数字
 *  - Boolean 布尔值
 *  - Array 数组，也可以用 [] 表示
 *  - Date 日期对象
 *  - Buffer Buffer 对象
 *  - Mixed 任意类型，需要使用 mongoose.Schema.Types.Mixed 指定
 *  - ObjectId 对象ID，需要使用 mongoose.Schema.Types.ObjectId 指定
 *  - Decimal128 高精度数字，需要使用 mongoose.Schema.Types.Decimal128 指定
 *
 * @AND
 * 字段类型验证
 * @brief 字段类型验证的格式如 vue 中 props 的验证格式相似
 */
var schema = new mongoose.Schema({
    name: {
        type: String,
        required: true, // 必须
        // 设置为唯一索引，若使用前集合存在记录且先前没有设置唯一索引则需要重建集合
        unique: true,
    },
    age: {
        type: Number,
        default: 0, // 默认值
    },
    gender: {
        type: String,
        enum: ["male", "female"], // 枚举：符合多选一
    },
    hobbies: Array,
    birthDate: Date,
    test: mongoose.Schema.Types.Mixed,
});
var Author = mongoose.model("author", schema);

/**
 * 当创建 Author 实例时所传入的对象中的属性值不满足规定字段类型时则会抛出字段类型错误
 */
var document = new Author({
    name: "test", // Path `name` is required
    age: 19,
    gender: "male",
    hobbies: ["smoke", "play", "run"],
    birthDate: new Date(),
    test: "test",
});
document.save().then((value) => {
    console.log(value);
    setTimeout(() => mongoose.disconnect(), 1000);
});

mongoose.connect("mongodb://127.0.0.1/trapdesign");
mongoose.connection
    .once("open", () => console.log("Open successfully"))
    .on("error", () => console.log("Connection error"))
    .on("close", () => console.log("Connection closed"));
```