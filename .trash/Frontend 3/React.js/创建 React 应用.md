通过将 React 作为标签引入到 HTML 中，或者通过脚手架这两种方式都可以创建 React 应用。

---

## 什么是 React 脚手架？

React 脚手架也称为 React 集成工具链。使用集成的工具链，以实现最佳的用户和开发人员体验。

使用 React 脚手架可以帮助开发人员快速创建一个基于 React 的模板项目。且有助于完成如下任务：

-   扩展文件和组件的规模。
-   使用来自 npm 的第三方库。
-   包含了所有需要的配置，包括语法检查、jsx 编译以及 devserver 等。
-   自动下载所有相关的依赖。
-   在开发中实时编辑 CSS 和 JavaScript 。
-   优化生产输出。
-   模块化、组件化以及工程化。

### 你可能不需要用到脚手架？

考虑把 React 作为普通的 `<script>` 标签添加到 HTML 页面上，以及使用可选的 JSX 编译器。通过 CDN 获得 React 和 ReactDOM 的 UMD 版本。

```jsx
<script crossorigin src="<https://unpkg.com/react@16/umd/react.development.js>"></script>
<script crossorigin src="<https://unpkg.com/react-dom@16/umd/react-dom.development.js>"></script>
```

上述版本仅用于开发环境，不适用于生产环境。压缩优化后可用于生产的 React 版本可通过如下方式引用：

```jsx
<script crossorigin src="<https://unpkg.com/react@16/umd/react.production.min.js>"></script>
<script crossorigin src="<https://unpkg.com/react-dom@16/umd/react-dom.production.min.js>"></script>
```

如果需要加载指定版本的 `react` 和 `react-dom` ，可以把其中的16替换成所需加载的版本号。

使用 Babel 进行 JSX 的语法编译。通过 CDN 获取 Babel。

```jsx
<script src="<https://unpkg.com/@babel/standalone/babel.min.js>"></script>
```

这是将 React 集成到现有网站最简单的方式。如果你认为更大的工具链有所帮助，可以随时添加！

### 推荐的工具链

React 团队主要推荐这些解决方案：

-   如果你在创建一个新的单页应用，使用 Create React App。
-   如果你是在用 Node.js 构建服务端渲染的网站，请使用 Next.js。
-   如果你是在构建面向内容的静态网站，试试 GATSBY。

---

## 创建 React 项目并运行

下面将分别介绍在 Linux 环境和 Windows 环境中创建并运行 React 项目。

### Linux 环境

1. 安装 npm ：`$ yay -S npm`
2. 安装 create 库：`$ sudo npm install -g create-react-app`
3. 切换到项目存放目录，创建一个 React 项目模板。创建命令：`$ create-react-app hello-react`
4. 进入项目文件夹，并启动项目：`$ cd hello-react && npm start`

### Windows 环境

1. 下载并安装 [Node.js](https://nodejs.org/en/) 。
2. 安装 create 库：`npm install -g create-react-app`
4. 切换到项目存放目录，创建一个 react 项目模板。创建命令：`$ create-react-app hello-react`
5. 进入项目文件夹，并启动项目：`$ cd hello-react && npm start`

>如果提示无法找到 npm 命令，则需要在 Windows 的环境变量中添加 nodejs 的路径：
>1. 执行组合键 `Win` + `Pause` 打开设置的关于页面。
>2. 将页面滚动到最下面，点击 “高级系统设置” 。
>3. 在系统属性对话框中点击 “环境变量” 。
>4. 在环境变量对话框中找到系统变量中的 “Path” ，双击 “Path” 编辑该环境变量。
>5. 点击右侧 ”新建“ 按钮，输入 nodejs 的存放路径，确定即可。
> ![[Pasted image 2023041300000.png]]

