## webpack 的基本使用

### 编写某个具体功能

1. 并运行 `> npm init -y` 命令，初始化包管理配置文件 `package.json`
2. 新建 `src` 源代码目录
3. 新建 `src/index.html` 以及 `src/index.js`

```html
<!-- index.html -->
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
	<li>6</li>
	<li>7</li>
	<li>8</li>
</ul>
```

```js
// index.js
import $ from "jquery"
$(function () {
  $("li:odd").css("background-color", "red")
  $("li:even").css("background-color", "pink")
})
```

4. 初始化首页基本的结构
5. 运行 `> npm install jquery -S` 命令，安装 jQuery

参数 -S 等价于 --save ，它表示明确将包名以及包版本信息记录至 `package.json` 文件中的节点 `dependecies` 中。但不加该参数 npm 也默认执行记录。

节点 `dependecies` 表示在项目上线时执行 `> npm install` 命令后必须下载安装的包

6. 通过 ES6 模块化的方式导入 jQuery，实现具体功能。`import $ from 'jquery';`

### 运行时出现错误

通过 import 关键字引入了 jQuery 库，但由于 ES 版本兼容性的问题导致浏览器不能够直接识别，需要借助 webpack 来解决此类问题。

![[前端开发/Webpack/attachments/Untitled 1.png]]

### 安装 webpack

执行命令 `> npm install webpack@5.42.1 webpack-cli@4.7.2 -D` 安装 webpack 以及 webpack 的 cli 。

> 参数 -D 等价于 --save-dev ，它表示明确将包名以及包版本信息记录至 `package.json` 文件中的节点 `devDependecies` 中

> 节点 `devDependecies` 表示在项目上线时执行 `> npm install` 命令后不需要下载安装的包。通过单词中的 dev 即可看出是开发环境下所需的依赖包。

### 配置 webpack

首先，在项目文件夹的根位置上创建一个 webpack 的配置文件 `webpack.config.js` 。

文件内容为：

```jsx
// 使用 Node.js 中的导出语法，向外导出一个 webpack 的配置对象。
module.exports = {
		// mode 用来指定 webpack 构建的模式。可选值为 development 和 production 。
    mode: "development",
}
```

<aside>
💡 production 模式用于在项目最终上线的时候，因为 webpack 在 production 模式下会进行代码压缩混淆、性能优化、删除注释等操作，以便于在上线的时候，JavaScript文件能够处于最小态。

</aside>

<aside>
💡 由于 production 模式下构建的时间会延长，因此不适用于开发环境下的编译，故有 development 和 production 之分。

</aside>

接着，我们还需要在 `package.json` 文件中的节点 “scripts” 下添加运行 webpack 的脚本。

添加内容如下：

```json
"scripts": {
  "dev": "webpack"  // dev 为脚本名称（可自行指定），对应了 webpack 命令。
},
```

最后，直接在终端运行 `> npm run dev` 即可启动 webpack ，webpack 被执行后将会对我们的项目打包构建。

![[Untitled 1 1.png]]

### 引入 webpack 生成的 JavaScript 文件

在之前创建的 `src/index.html` 中引入 webpack 生成的 JavaScript 文件以显示页面效果。

```html
<head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- <script src="./index.js"></script> -->
    <script src="../dist/main.js"></script>
    <title>index</title>
</head>
```

<aside>
💡 默认地，webpack 会将最终生成的 JavaScript 文件保存在根目录下 `dist` 文件夹中。文件名默认为 `main.js` 。

</aside>

可以看到实现了页面效果

![[前端开发/Webpack/attachments/Untitled 2.png]]

## webpack 的输入输出源

### 默认输入和输出源

在 webpack `4.x` 和 `5.x` 的版本中，对于输入和输入源如下默认约定：

- 输入源：`src/index.js`
- 输出源：`dist/main.js`

输入和输出源可以在 `webpack.config.js` 文件中指定（修改）。

### 自定义输入和输出源

在 `webpack.config.js` 配置文件中，通过 `entry` 节点指定构建的入口（输入源）；通过 `output` 节点指定构建的出口（输入源）。

```jsx
// 引入 Node.js 的 path 模块。
const path = require('path');

module.exports = {
		// 指定构建模式。
		mode: "development",
		// 指定构建输入文件。
		entry: path.join(__dirname, "./src/index.js"),  // __dirname 表示当前文件所在目录
		// 指定构建输出文件
		output: {
				path: path.join(__dirname, "./dist"),  // 指定输出文件的存放路径
				filename: 'main.js',  // 指定输出文件的文件名
		}
}
```