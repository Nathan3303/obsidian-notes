## webpack-dev-server 的简介

### 使用 webpack-dev-server 的原因

在[【webpack 基础】](webpack%20基础.md)一文中，我们提到了如何安装与配置 webpack ，如何运行 webpack 对我们的项目进行编译构建，通过定义在 `package.json` 里的一个脚本，即运行命令 `> npm run dev` 来使 webpack 工作，但当我们修改了源代码，我们不得不再次运行上面的命令，再次通过 webpack 编译以达到最新的效果实现，这样一来就会比较麻烦，不利于开发者编码。因此，通过 webpack-dev-server 这个插件可以解决这个问题，该插件会实时地监听源代码的变动，在开发者做出修改（`Ctrl+S`）后自动执行 webpack 的构建，提高开发效率。

总体来说，webpack-dev-server ：

- 类似于 Node.js 的 nodemon 工具。
- 会在源代码被修改后，自动地进行项目的打包和构建。

## webpack-dev-server 的使用

### 安装 webpack-dev-server

运行命令 `> npm install webpack-dev-server@3.11.2 -D` 安装指定插件。

### 配置 package.json

编辑 package.json 文件，修改 `scripts` 节点，将原有的 `webpack` 命令更改为 `webpack serve` 命令。

修改后内容如下：

```json
"scripts": {
		// "dev": "webpack" (OLD)
		"dev": "webpack serve"
},
```

### 运行 webpack-dev-server

运行命令 `> npm run dev` 以启动 webpack-dev-server 。

![[前端开发/Webpack/attachments/Untitled 4.png]]
webpack-dev-server 启动终端反馈截图

根据上图可以看出，在 webpack-dev-server 启动后，会开启一个 http 服务器，并且在第一次 webpack 执行完成后不会立即退出，将持续监听源代码变动并作出响应。

### 在启动 webpack-dev-server 时遇到问题

在执行命令 `> npm run dev` 后出现错误 `Unable to load '@webpack-cli/serve' command` 。

![[Untitled 1 2.png]]
运行出错截图

这种情况下我们只需重新安装 webpack-cli 即可，运行命令 `> npm i webpack-cli -D` 安装。

## 使用 webpack-dev-server 的注意事项

在启动了 webpack-dev-server 后，会开启一个 http 服务器，并且终端程序会进行源代码变更的监听，在源代码更新后会自动做出构建响应，因此我们需要了解 webpack-dev-server 的基本原理，来使我们能够正确地使用到 webpack-dev-server 。

### 如何访问到更新后的页面？

上面说到 webpack-dev-server 会启动一个 http 服务器，通过访问这个服务地址就可以访问到我们的项目了。

要注意在启动 webpack-dev-server 后终端的一些输出信息：

![[Untitled 2 1.png]]

其中：第一至三行，输出了 http 的服务地址，一般是通过环回和本机 IP 地址进行访问，访问的端口默认设置为 8080 ；第四行的输出则表示当前 http 服务器的服务目录，即访问服务地址后直接访问该目录。

按照规范化的约定，public 目录下存放的是网站（单个应用）的静态文件，如网站 HTML 入口文件、CSS 文件以及网站的偏爱图标等。因此，我们需要按照输出的路径去创建一个 public 文件夹（如果没有 public 目录），将 `index.html` 入口文件移动到 public 下即可。移动之后，当我们去访问服务地址，浏览器会自动地去访问 `index.html` 文件。

![[Untitled 3 1.png]]

### 输出文件如何引入？

在[【webpack 基础】](webpack%20基础.md)一文中说到，webpack 会在编译后生成一个输出文件，默认地会输出为 `dist/main.js` 文件。

但是通过 webpack-dev-server 插件进行编译后的输出文件并不会保存到 dist 下，而是会保存在内存中，那么我们要如何访问到这个编译后的 JavaScript 文件，从而引入到 index.html 中去？

webpack-dev-server 将该文件的引用设置在了项目文件夹的根 `/` 路径下，并且由于是存在内存中因此无法通过资源管理器查看到。若没有修改 webpack 的输出设置项，那么输出文件的引入路径为 `/main.js` 。

接下来修改 index.html 即可：

```html
<head>
		<meta charset="UTF-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<!-- <script src="./index.js"></script> -->
		<script src="/main.js"></script>
		<title>index</title>
</head>
```

## 可选设置

### 在第一次编译后自动开启浏览器并访问服务地址

修改 `webpack.config.js` 配置文件，在配置对象中添加 `devServer` 节点以配置 webpack-dev-server 。

添加内容如下：

```jsx
devServer: {
		open: true,  // 首次编译后是否自动开启浏览器 (true|false)。
		host: '127.0.0.1',  // 服务地址（默认为 localhost）
		port: 80,  // 服务端口号（默认为 8080）
}
```

<aside>
💡 在修改了 `webpack.config.js` 后需要停止终端正在运行的 webpack-dev-server 并重新启动，才会加载修改后的配置文件。

</aside>