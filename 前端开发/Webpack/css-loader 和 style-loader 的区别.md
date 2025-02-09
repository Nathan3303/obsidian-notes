
由于 webpack 是使用 JavaScript 所编写，运行在 Node.js 环境中，所以默认地 webpack 在打包构建的时候只会去处理各 JavaScript 模块之间的依赖关系。

因为像 CSS 文件不是一个有效的 JavaScript 模块，我们就需要配置 webpack loader ，使用 css-loader 和 style-loader 去合理地处理他们。

## css-loader

### 关于 css-loader 的作用

如果在某个 JavaScript 模块中引入了 CSS 文件，那么我们就需要使用 css-loader 来识别这个 CSS 的模块文件，通过特定的语法转换将内容导出。

> css-loader 会处理通过 `import` / `require()` / `@import(url)` 引入的内容。
> 

### 关于 css-loader 的处理结果

现设有 `index.css` 和 `index.js` 两个文件，文件内容如下：

```css
.unorder-list {
		width: 50%;
		margin: 0 auto;
		list-style: none;
}
```

```jsx
const cssLoaderRes = require('./index.css');
console.log(cssLoaderRes, 'css');
```

运行命令 `> npm run dev` 启动 webpack-dev-server ，访问服务地址，可以看到在控制台上输出了 css-loader 处理之后的返回值。

![[前端开发/Webpack/attachments/Untitled.png]]

通过上图我们可以看到经过 css-loader 处理后返回的是一个模块对象，这个对象里面有一个数组记录这 CSS 文件的内容。

但这并不是我们想要的，因为这个模块对象无法给到浏览器进行 CSS 渲染。此时，我们就需要用到另外的一个 loader ，即 style-loader 来进行处理。

这也是我们为什么用了两个 loader 来进行 CSS 文件的完整构建。

下面将介绍 style-loader 。

## style-loader

### 关于 style-loader 的作用

style-loader 是通过一个 JavaScript 脚本来创建一个 style 标签，然后将内容写入到标签里面，而这个内容就来自于 css-loader 。

因此 style-loader 是不能够单独使用的，因为 style-loader 没有办法解析 import 等关键字，不能够解析 CSS 之前的依赖关系。

由于每个 loader 的功能都是单一的，各自拆分独立，所以我们需要两个 loader 去完成对 CSS 文件的打包构建。