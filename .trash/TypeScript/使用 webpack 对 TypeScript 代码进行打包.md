## 初始化项目

通过 npm 命令生成 package.json 文件
执行命令：`npm init`，填写项目名称后一路回车即可。

## 安装 webpack 以及 TypeScript 的相关包

1. 安装 TypeScript：`npm install typescript`
2. 安装 webpack：`npm install -D webpack webpack-cli`
3. 安装 ts-loader：`npm install -D ts-loader`

## 创建并编辑 webpack 的配置文件

在项目根目录中创建 webpack.config.js 文件，并在该文件中填写 webpack 的配置信息

```javascript
// 引入 path 模块
const path = require("path");

// webpack 配置对象
module.exports = {
    // 指定入口文件（主文件）
    entry: "./src/index.ts",

    // 指定输出文件路径和文件名
    output: {
        // 路径
        path: path.resolve(__dirname, "dist"),
        // 文件名
        filename: "bundle.js",
        // 配置打包环境
        environment: {
            // 关闭打包输出时外层立即函数使用箭头函数
            arrowFunction: false,
        },
    },

    // 指定需使用的模块
    module: {
        // 编辑加载规则
        rules: [
            // 设置 ts-loader
            {
                // 指定规则生效的文件（正则表达式）
                test: /\.ts$/,
                // 指定 loader
                use: "ts-loader",
                // 设置排除的文件或目录
                exclude: /node_modules/,
            },
        ],
    },

    // 设置 webpack 的打包模式
    mode: "development",

    // 模块化配置
    resolve: {
        // 告知 webpack .ts 以及 .js 文件能够作为模块文件使用
        extensions: [".ts", ".js"],
    },
};
```

## 创建并编辑 TypeScript 的配置文件

同样在项目根目录创建 tsconfig.json 文件，并填写 TypeScript 的配置以及编译选项

```json
{
    // "include": ["./src/**/*"],
    // "exclude": ["./src/assets/*"],
    "compilerOptions": {
        "target": "es2015",
        "module": "es2015",
        "strict": true
    }
}
```

## 在 package.json 中添加 webpack 的打包命令

在 package.json 中的 "scripts" 中添加指令 `"build": "webpack"`

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
},
```

## 实现 webpack 自动生成静态的 html 文件

在能够实现 webpack 自动打包 TypeScript 代码后，我们需要手动创建一个 html 文件，然后在这个 html 文件
中引入 webpack 打包后的 ./dist/bundle.js 文件才能够应用这些代码。这里我们可以通过 webpack 的插件自动
生成这样的 html 文件，不需要手动创建

### 安装 html-webpack-plugin

在命令行中执行 `npm install -D html-webpack-plugin` 安装插件

### 编辑 webpack.config.js

引入 html-webpack-plugin 。在 webpack.config.js 中添加如下代码：

```javascript
/* ... */
// 引入 html-webpack-plugin
const HTMLWebpackPlugin = require("html-webpack-plugin");
/* ... */
module.exports = {
    /* ... */
    // 新增 plugins 属性
    plugins: [
        // 引入插件
        new HTMLWebpackPlugin(),
    ],
    /* ... */
};
```

## 实现项目在开发环境中的自动打包

在上一节我们通过配置 webpack 的插件 html-webpack-plugin 实现了能够在打包时自动生成
静态入口文件 index.html，我们需要在浏览器中访问这个 html 文件来运行 TypeScript 编译
后的 JavaScript 代码，查看其运行效果。

在开发环境中，通过浏览器直接访问不够优雅，我们在代码发生变化时需要手动调用 `npm run build`
指令再次打包新的代码，打包完成后需要在浏览器中手动刷新才能够获取到最新的代码，显示最新的运行
效果，因此需要配置项目开发环境，使其能够在代码发生变化时自动重新打包，并且能够自动刷新页面。

### 安装 webpack-dev-server

在命令行中执行 `npm install -D webpack-dev-server` 安装软件包

### 在 package.json 中增加新的命令

在 package.json 中的 "scripts" 中添加指令 `"start": "webpack serve --open"`

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack serve --open"
},
```

### 启动 webpack-dev-server

在命令行中执行 `npm run start` 启动服务。

## 实现 dist 目录的清除

避免 dist 出现覆盖错误，引起需要在每次编译后文件输出前，先清空 dist 目录中的所有内容，然后在生成新的内容

### 安装 clean-webpack-plugin

在命令行中执行 `npm install -D clean-webpack-plugin` 安装软件包

### 编辑 webpack.config.js

引入 clean-webpack-plugin 。在 webpack.config.js 中添加如下代码：

```javascript
/* ... */
// 引入 clean-webpack-plugin
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
/* ... */
module.exports = {
    /* ... */
    plugins: [
        // 引入插件
        new CleanWebpackPlugin(),
        /* ... */
    ],
    /* ... */
};
```

## 引入 babel 和 core-js

babel 支持将高版本的 JavaScript 代码转换为低版本的 JavaScript 代码，从而兼容低版本的浏览器
并且 babel 还支持其他的一些高级语法的转换，比如 JSX 语法等。

### 安装 babel

1. 安装 babel 核心：`npm install -D @babel/core`
2. 安装 babel 预设：`npm install -D @babel/preset-env`
3. 安装 babel-loader：`npm install -D babel-loader`
4. 安装 core-js：`npm install -D core-js`

> 同时安装：`npm install -D @babel/core @babel/preset-env babel-loader core-js`

### 编辑 webpack.config.js

在 webpack.config.js > module > rules 属性中添加 babel-loader 的设置

```javascript
/* ... */
module.exports = {
    module: {
        rules: [
            {
                test: /\.ts$/,
                // 修改 use 中的内容：添加对 babel-loader 的支持
                use: [
                    {
                        // 指定加载器
                        loader: "babel-loader",
                        // 配置 babel
                        options: {
                            // 设置预定义环境
                            presets: [
                                [
                                    // 指定环境的插件
                                    "@babel/preset-env",
                                    // 配置信息
                                    {
                                        // 要兼容的目标浏览器
                                        targets: { chrome: "88", ie: "11" },
                                        // 指定 core-js 的版本
                                        corejs: "3",
                                        // 指定使用 core-js 的方式为 usage 表示按需加载
                                        useBuiltIns: "usage",
                                    },
                                ],
                            ],
                        },
                    },
                    "ts-loader",
                ],
                exclude: /node_modules/,
            },
        ],
    },
};
/* ... */
```
