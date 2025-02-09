Vue **不支持** IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有[兼容 ECMAScript 5 的浏览器](https://caniuse.com/#feat=es5)。

Vue2 的最新版本为 2.7.14，每个版本的更新日志见 [GitHub](https://github.com/vuejs/vue/releases) 。

Vue 2.7 是当前、同时也是最后一个 Vue 2.x 的次级版本更新。Vue 2.7 会以其发布日期，即 2022 年 7 月 1 日开始计算，提供 18 个月的长期技术支持 (LTS：long-term support)。在此期间，Vue 2 将会提供必要的 bug 修复和安全修复，但不再提供新特性。

**Vue 2 的终止支持时间是 2023 年 12 月 31 日**。在此之后，Vue 2 在已有的分发渠道 (各类 CDN 和包管理器) 中仍然可用，但不再进行更新，包括对安全问题和浏览器兼容性问题的修复等。

---

## Vue Devtools（开发工具）

在使用 Vue 时，我们推荐在浏览器上安装 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools) 。它允许你在一个更友好的界面中审查和调试 Vue 应用。

---

## 通过 `<script>` 标签引入 Vue（CDN）

直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量。

需要注意的是在开发环境中不要引入生产环境的 Vue ，否则会失去所有常见错误的相关警告。

```html
<!-- 对于制作原型或学习，你可以这样使用最新版本 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
```

```html
<!-- 对于生产环境，推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14"></script>
```

```html
<!-- 如果你使用原生 ES Modules，这里也有一个兼容 ES Module 的构建文件 -->
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.esm.browser.js'
</script>
```

-   开发版本：[https://v2.cn.vuejs.org/js/vue.js](https://v2.cn.vuejs.org/js/vue.js)
-   生产版本：[https://v2.cn.vuejs.org/js/vue.min.js](https://v2.cn.vuejs.org/js/vue.min.js)

请确认了解[不同构建版本](https://v2.cn.vuejs.org/v2/guide/installation.html#%E5%AF%B9%E4%B8%8D%E5%90%8C%E6%9E%84%E5%BB%BA%E7%89%88%E6%9C%AC%E7%9A%84%E8%A7%A3%E9%87%8A)并在你发布的站点中使用**生产环境版本**，把 `vue.js` 换成 `vue.min.js` 。这是一个更小的构建，可以带来比开发环境下更快的速度体验。

---

## 在构建工具中安装 Vue（NPM）

在用 Vue 构建大型应用时推荐使用 NPM 安装。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件](https://v2.cn.vuejs.org/v2/guide/single-file-components.html)。

```bash
# 最新稳定版
npm install vue
```

### Vue CLI

Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用 (SPA) 快速搭建繁杂的脚手架。它为现代前端工作流提供了开箱即用的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本。更多详情可查阅 [Vue CLI 的文档](https://cli.vuejs.org/)。

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

 - 通过 `@vue/cli` 实现的交互式的项目脚手架。
 - 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。
 - 一个运行时依赖 (`@vue/cli-service`)，该依赖：
	* 可升级；
	- 基于 webpack 构建，并带有合理的默认配置；
	- 可以通过项目内的配置文件进行配置；
	- 可以通过插件进行扩展。
 - 一个丰富的官方插件集合，集成了前端生态中最好的工具。
 - 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

通过 Vue CLI 创建一个 Vue 项目。[https://cli.vuejs.org/zh/guide/creating-a-project.html](https://cli.vuejs.org/zh/guide/creating-a-project.html)

```powershell
# 安装 Vue CLI
npm install -g @vue/cli

# 创建 Vue 工程
vue create my-project
```

💡 Vue CLI 现已处于维护模式，目前官方推荐使用 [create-vue](<https://github.com/vuejs/create-vue>) 来创建基于 [Vite](https://cn.vitejs.dev/) 的新项目。

### Vite

通过官方推荐使用的 create-vue 来创建基于 Vite 的 Vue 项目。

[Vite](https://cn.vitejs.dev/) 是一个轻量级的、速度极快的构建工具，对 Vue SFC 提供第一优先级支持。

```shell
npm init vue@latest
```

这个命令会安装和执行 [create-vue](https://github.com/vuejs/create-vue)，它是 Vue 提供的官方脚手架工具。跟随命令行的提示继续操作即可。

- 更多关于 Vite 的知识：[Vite 官方文档](https://cn.vitejs.dev)。
- 若要了解如何为一个 Vite 项目配置 Vue 相关的特殊行为，比如向 Vue 编译器传递相关选项，请查看 [@vitejs/plugin-vue](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue#readme) 的文档。