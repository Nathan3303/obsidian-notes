## loader 简介

loader ，中文是 “加载器” 。在实际开发过程中，webpack 默认地只能够打包 JavaScript 文件，即以 .js 为后缀的模块文件，其他非 .js 后缀的文件则无法处理，需要对应的 loader（加载器）去进行构建（代码压缩混淆、性能优化等）的处理。

比如 css 相关文件（.css后缀）、less 相关文件（.less后缀）以及 [JSX 文件](前端开发/React/JSX%20简介.md)（.jsx后缀）等，就需要借助相对应的 loader 进行处理。

## webpack 调用 loader 的过程

下图是 webpack 判断并调用对应 loader 的流程图：

![[前端开发/Webpack/attachments/Untitled 3.png]]

## 在 webpack 中配置 css-loader

有了 webpack ，我们就可以在 JavaScript 文件中通过 import 关键字的方式引入一个 CSS 文件。

```jsx
import './index.css';
```

那么相对应地，我们也需要为 webpack 配置相对应的 loader ，即为 css-loader 。否则 webpack 将无法处理这条语句。

> 当我们没有配置相关的 loader 去处理对应文件时， webpack 会抛出错误：“You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file...” 。

接下来我们将接着在[【webpack-dev-server】](webpack-dev-server.md)一文中的项目，来完成在 webpack 中 css-loader 的安装与配置。

### 安装 style-loader 和 css-loader

运行命令 `> npm install style-loader css-loader -D` 以安装 loader 包。

> 关于 style-loader 和 css-loader 各自的功能，以及这两个 loader 是如何合作完成文件构建的，可以查看 [css-loader 和 style-loader 的区别](css-loader%20和%20style-loader%20的区别.md) 一文。

### 添加 loader 的匹配规则

编辑 `webpack.config.js` ，在 webpack 配置对象中添加 `module` 节点，并且在 `module` 节点下添加匹配规则的设置 `rule` 。

以下是添加的具体内容：

```jsx
// 构建第三方模块文件的相关配置
module: {
		// 指定模块文件的匹配规则
		rule: [
				// test 表示模块文件后缀的匹配形式。值为一个正则表达式。
				// use 表示模块文件匹配成功，使用什么进行处理。值为一个数组，允许包含多个 loader 。
				{ test: /\.css$/, use: ["style-loader", "css-loader"] }
		],
},
```

要注意的是 `module` 节点与之前的 `devServer` 节点是平级的。

上述内容添加完成后，现在整个 webpack 的配置文件 `webpack.config.js` 的内容应当如下：

```jsx
const path = require("path");

module.exports = {
    mode: "development",
    entry: path.join(__dirname, "./src/index.js"),
    output: {
        path: path.join(__dirname, "./dist"),
        filename: "main.js",
    },
    devServer: {
        open: true,
        port: 8080,
    },
    module: {
        rule: [
            { test: /\.css$/, use: ["style-loader", "css-loader"] }
        ],
    },
};
```

## 关于在 webpack 中多个 loader 的调用顺序

在上述配置中，一个规则同时对应了两个 loader ，那么 webpack 是以什么样的顺序去调用这些 loader 的？

实际上 webpack 无法自动识别多个 loader 的理想调用顺序，只能通过配置文件去识别。

比如我们上面写入的规则：

```jsx
rule: [
    { test: /\.css$/, use: ["style-loader", "css-loader"] }
],
```

在 loader 数组中，webpack 会按照从后往前的顺序去调用 loader 以处理对应的引入。

那么按照上述配置，webpack 会先去调用 css-loader ，然后才是 style-loader 。

因此我们需要正确认识两个 loader 之间的关系，以把正确的调用顺序给到 webpack 进行 loader 的调用。

## 在 webpack 中配置 babel-loader

### 安装 babel-loader

运行命令 `> npm install babel-loader @babel/core -D`

### 配置 webpack 匹配规则

在 webpack.config.js 的 module->rules 节点数组中，添加 loader 的规则如下：

```jsx
{
    test: /\.js$/,  // 匹配以 .js 或 .jsx 后缀结尾的类型文件。
    use: "babel-loader",  // 匹配成功后通过 babel-loader 进行处理。
    exclude: /node_modules/,  // 该项用于指定排除不需要匹配的目录。
}
```

## 总结

1. webpack 默认只能够处理 JavaScript 模块之间的依赖关系。
2. 由于在 JavaScript 模块中引入的 CSS 文件，而 webpack 无法识别 CSS 代码，因此webpack 无法直接完成构建。
3. webpack 无法直接构建，则会寻找相对应的 loader 去处理。此时 loader 的规则需要被正确配置在 `webpack.config.js` 文件中。
4. 根据给定的 loader 调用顺序：
    1. 调用 css-loader 处理 CSS 文件，并保存 css-loader 处理后的返回值。
    2. 调用 style-loader 继续处理 css-loader 的返回值，将处理好的 JavaScript 代码保存。
5. 把 loader 处理后的代码，同输入源的代码做合并，打包到输出文件中。