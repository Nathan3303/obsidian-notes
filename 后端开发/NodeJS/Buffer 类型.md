
官方文档中的 `Buffer` 章节页：  
https://nodejs.cn/dist/latest-v18.x/docs/api/buffer.html#%E7%BC%93%E5%86%B2%E5%8C%BA

## `Buffer` 简述

`Buffer` 对象用于表示固定长度的字节序列，中文称为缓冲区，许多 Node.js API 都支持 `Buffer` 。比如 `fs` 模块中的 `readFileSync` API，该 API 返回的数据类型即为 `Buffer` 。下面是 `readFileSync` 函数的原型之一：

```javascript
function readFileSync(
    path: fs.PathOrFileDescriptor,
    options?: {
        encoding?: null | undefined;
        flag?: string | undefined;
    } | null | undefined
): Buffer (+2 overloads)
```

`Buffer` 对象用于表示固定长度的字节序列。也就是说，`Buffer` 类似于定长的 “数组”。且 `Buffer` 类是 JavaScript `Uint8Array` 类的子类，由此可以得出：`Buffer` 这样一个定长 “数组” 中的每个元素都是固定一个字节的长度。得益于 `Buffer` 这样的数据类型，使得能够直接处理二进制数据，因此 `Buffer` 的性能会相对较好。

`Buffer` 能够在全局作用域内使用，但官方文档建议通过 `import` 或 `require` 语句显示的引用它。

```javascript
import { Buffer } from "node:buffer";
// or
const buffer = require("buffer");
```

## `Buffer` 的基本使用

### 创建 `Buffer` 对象的三种方式

#### 使用 `alloc` API 创建一个 `Buffer` 对象

```javascript
let buf = Buffer.alloc(10);
console.log(buf);
// <Buffer 00 00 00 00 00 00 00 00 00 00>
```

#### 使用 `allocUnsafe` API 创建一个 `Buffer` 对象

需要注意 `allocUnsafe` API 有别于 `alloc` API，在通过 `allocUnsafe` API 创建一个 `buffer` 对象时，会将该对象中的所有的元素置零，也就是对象所对应的内存区域置零。

```javascript
let buf2 = Buffer.allocUnsafe(10);
console.log(buf2);
// <Buffer 00 00 00 00 00 00 00 00 00 00>
```

#### 使用 `allocUnsafe` 创建一个 `Buffer` 对象

在使用 `from` 这个 API 转换中文字符串时，每个中文字符需要使用三个字节的长度。

```javascript
let buf3 = Buffer.from("hello"); // 转英文字符串
console.log(buf3);
// <Buffer 68 65 6c 6c 6f>

let buf4 = Buffer.from("你好"); // 转中文字符串
console.log(buf4);
// <Buffer e4 bd a0 e5 a5 bd>

let buf5 = Buffer.from([108, 111, 118, 101]); // 转数组
console.log(buf5);
// <Buffer 6c 6f 76 65>
```

### `Buffer` 对象转换为字符串

通过使用上面的变量，将变量 `buf4` 转换为字符串并输出。

```javascript
console.log(buf4.toString()); // 默认UTF-8
```

> toString API 的默认转换编码为 UTF-8，也可以通过传入一个编码类型指定 toString 的转换形式。例如 `console.log(buf4.toString('hex'));`

### 操作（修改）`Buffer` 中的元素

```javascript
console.log(buf3, buf3[0]);
// <Buffer 68 65 6c 6c 6f> 104

buf3[0] = 95;

console.log(buf3, buf3.toString());
// <Buffer 5f 65 6c 6c 6f> _ello
```
