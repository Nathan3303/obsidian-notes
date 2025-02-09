
在 NPM 包的 `dist/` 目录你将会找到很多不同的 Vue.js 构建版本。

![[Pasted image 20230413215425.png]]

下表列出了它们之间的差别：

![[Pasted image 20230413215348.png]]

---

## 相关术语解释

-   **完整版**：同时包含编译器和运行时的版本。
-   **编译器**：用来将模板字符串编译成为 JavaScript 渲染函数的代码。
-   **运行时**：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的代码。基本上就是除去编译器的其它一切。
-   **[UMD](https://github.com/umdjs/umd)**：UMD 版本可以通过 `<script>` 标签直接用在浏览器中。jsDelivr CDN 的 [https://cdn.jsdelivr.net/npm/vue@2.7.14](https://cdn.jsdelivr.net/npm/vue@2.7.14) 默认文件就是运行时 + 编译器的 UMD 版本 (`vue.js`)。
-   **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**：CommonJS 版本用来配合老的打包工具比如 [Browserify](http://browserify.org/) 或 [webpack 1](https://webpack.github.io/)。这些打包工具的默认文件 (`pkg.main`) 是只包含运行时的 CommonJS 版本 (`vue.runtime.common.js`)。
-   **[ES Module](http://exploringjs.com/es6/ch_modules.html)**：从 2.6 开始 Vue 会提供两个 ES Modules (ESM) 构建文件：
	- 为打包工具提供的 ESM：为诸如 [webpack 2](https://webpack.js.org/) 或 [Rollup](https://rollupjs.org/) 提供的现代打包工具。ESM 格式被设计为可以被静态分析，所以打包工具可以利用这一点来进行“tree-shaking”并将用不到的代码排除出最终的包。为这些打包工具提供的默认文件 (`pkg.module`) 是只有运行时的 ES Module 构建 (`vue.runtime.esm.js`)。
	- 为浏览器提供的 ESM (2.6+)：用于在现代浏览器中通过 `<script type="module">` 直接导入。

---

## 运行时+编译器 vs. 只包含运行时

如果你需要在客户端编译模板 (比如传入一个字符串给 `template` 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板)，就将需要加上编译器，即完整版：

```jsx
// 需要编译器
new Vue({
	template: '<div>{{ hi }}</div>'
})
```

```jsx
// 不需要编译器
new Vue({
	render (h) {
		return h('div', this.hi)
	}
})
```

当使用 `vue-loader` 或 `vueify` 的时候，`*.vue` 文件内部的模板会在构建时预编译成 JavaScript。

你在最终打好的包里实际上是不需要编译器的，所以只用运行时版本即可。