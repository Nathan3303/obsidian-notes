## 创建一个 Vue 实例

通过 Vue.js 提供的构造函数 Vue 以创建一个 Vue 实例。每个 Vue 应用都是通过这个实例开始的。

```js
const vm = new Vue();
```

虽然没有完全遵循 [MVVM 模型](https://baike.baidu.com/item/MVVM/96310?fr=aladdin)，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 `vm` (ViewModel 的缩写) 这个变量名表示 Vue 实例。

在创建一个 Vue 实例时，需要传入配置对象，该配置对象用于创建指定的特性和行为。

---

## 数据定义（data 选项）

当一个 Vue 实例被创建时，其配置对象中 `data` 选项中的所有的 property 都加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```js
// 数据对象
const ourData = { a: 1, b: 2 };

// 将上面定义的数据对象加入到 Vue 实例中
const vm = new Vue({
	data: ourData,  // 加入响应式系统
})
```

上述代码将一个定义在外部的数据通过 data 配置项传入到了 Vue 实例中，Vue 在内部会对 data 的值进行一个 [数据代理](obsidian://open?vault=Nathans&file=Vue.js%2FVue2%2F%E5%93%8D%E5%BA%94%E5%BC%8F%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86) ，劫持获取和修改操作。


当 Vue 实例 vm 中的代理对象的值发生变化时，则会影响到原始数据：

```js
console.log(data.a)  // => 1
vm.a = 2
console.log(data.a)  // => 2
```

反之亦然：

```js
console.log(vm.b)  // => 2
data.b = 1
console.log(vm.b)  // => 1
```

若当模板中有使用到相关数据的地方，则 Vue 会在数据代理对象劫持到修改操作时重新渲染模板，对用到该数据的地方进行更新。这也就完成了一个完整的数据和视图的双向数据绑定。

值得注意的是只有当实例被创建时就已经存在于 `data` 中的 property 才是**响应式**的。也就是说如果你添加一个新的 property，比如：

```js
vm.c = 3
```

此时实例 vm 的自身确实增加了一个 property `c`，但是这样的改动并不会触发任何的视图更新，因为他并没有通过 响应式系统[^basic-reactive] 的包装，数据代理对象无法劫持该 property 的变化。

如果需要在晚些时候应用到某些 property，可以将他们提前定义到 `data` 配置项中，并赋予初始值，如：

```js
const vm = new Vue({
	data: {
		ourData,
		p1: null,
		p2: "Hello, Vue.js",
	},
})
```

### 查看当前 vm 的数据对象

```js
// 数据对象
const ourData = { a: 1, b: 2 };

// 将上面定义的数据对象加入到 Vue 实例中
const vm = new Vue({
	data: ourData,  // 加入响应式系统
})

// 查看 vm 中的数据对象
const res = vm.$data === ourData;
console.log(res);  // true
```

---

## 容器绑定（el 选项）

在创建 Vue 实例后，vm 是如何知道数据更新的地方？是整个 HTML 结构还是其中的某个元素？当然 vm 可以默认就检索整个 HTML 结构，当发现其中有 Mustache 标签、指令、过滤器等时进行处理，但 vm 并不会这么做，所以我们需要在实例化 Vue 时通过配置项 `el` 对 vm 进行容器绑定，指定该 vm 所管理的容器。

`el` 配置项的值必须是一个 `id` 选择器，否则容器不唯一时 vm 是无法工作的。

### 通过配置项绑定

```html
<!-- 引入 Vue.js -->
<body>
	<div id="root">{{ name }}</div>
	<div id="app">{{ name }}</div>
</body>
<script>
	const vm1 = new Vue({
		el: "#root",  // 将 vm1 与容器 #root 绑定
		data: {
			name: "vm1",
		},
	});
	const vm2 = new Vue({
		el: "#app",  // 将 vm2 与容器 #app 绑定
		data: {
			name: "vm2",
		},
	})
</script>
```

在上述例子中，虽然两个容器模板中 Mustache 标签所引用的 property 一致，但两个容器各自受到不同的 vm 所管理，因此渲染出来的值不一致。

### 通过调用 `$mount` 方法绑定

除了通过在实例化时传入配置项的方式进行容器绑定，还可以通过 vm 中的 `$mount` 方法进行容器绑定。

```html
<!-- 引入 Vue.js -->
<body>
	<div id="root">{{ name }}</div>
</body>
<script>
	const vm = new Vue({
		data: {
			name: "Hello, Vue.js",
		},
	});

	vm.$mount("#root");  // 将 vm 与容器 #root 绑定
</script>
```

### 查看当前 vm 所绑定的容器

```js
// 绑定容器
const vm = new Vue({ el: "#root", })

// 查看容器
console.log(vm.$el)
const res = vm.$el === document.getElementById("#root");
console.log(res)  // true
```

---

## 实例的生命周期及其钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

### 生命周期及其钩子图示

![Vue 实例生命周期](https://v2.cn.vuejs.org/images/lifecycle.png)

[^basic-reactive]: Vue2中响应式系统的基本原理 to [响应式基本原理](obsidian://open?vault=Nathans&file=%E5%89%8D%E7%AB%AF%E7%AC%94%E8%AE%B0%2FVue.js%2FVue2%2F%E5%93%8D%E5%BA%94%E5%BC%8F%E7%B3%BB%E7%BB%9F%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%AE%9E%E7%8E%B0)
