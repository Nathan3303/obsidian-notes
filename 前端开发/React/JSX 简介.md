
现有变量声明如下：

```jsx
const element = <h1>Hello, world!</h1>;
```

在上述代码中，常量 element 的值既不是字符串也不是 HTML 标签。

它被称为 JSX，是一个 JavaScript 的语法扩展。我们建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它具有 JavaScript 的全部功能。

JSX 可以生成React”元素“。在下一章节我们将探讨如何如何讲这些元素渲染到页面上。

## 为什么使用JSX？

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现[*关注点分离*](https://baike.baidu.com/item/%E5%85%B3%E6%B3%A8%E7%82%B9%E5%88%86%E7%A6%BB/7515217?fr=aladdin)。

> 关注点分离（Separation of concerns，SOC）
> 
> 
> 关注点分离是面向对象的程序设计的核心概念。分离关注点使得解决特定领域问题的代码从业务逻辑中独立出来，业务逻辑的代码中不再含有针对特定领域问题代码的调用(将针对特定领域问题代码抽象化成较少的程式码，例如将代码封装成function或是class)，业务逻辑同特定领域问题的关系通过侧面来封装、维护，这样原本分散在在整个应用程序中的变动就可以很好的管理起来。
> 

React 不强制要求使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

## 在JSX中嵌入表达式

现有代码如下：

```jsx
const name = 'Nathan Lee';
const element = <h1>hello, {name}</h1>;

ReactDOM.render(
	element,
	document.getElementById('root')
)
```

在上述代码中，声明了一个姓名常量，并且在常量 element 中通过一对大括号的形式应用了这个姓名常量。

> 在使用 ReactDOM 时需要进行引入操作。
> 
> 
> ```jsx
> // 通过CND方式获取ReactDOM
> <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
> 
> // 通过import方法获取（React工具链）
> import ReactDOM from "react-dom";
> ```
> 
> `ReactDOM.render` 函数接收两个参数：渲染元素以及被渲染元素。
> 

在JSX语法中，可以在大括号内任何有效的 JavaScript 表达式。例如数值计算（`2+2`）、函数调用（`formatName(user)`）、变常量调用（`name`），成员调用（`user.name`）等都是有效的JavaScript 表达式。

下面是较为复杂的例子：

```jsx
function formatName(user) {
	return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Nathan',
  lastName: 'Lee'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
	</h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

## JSX的特定属性

可以通过使用引号来设置元素属性的字面量。

```jsx
const element = <div tabIndex="0"></div>;
```

也可以使用大括号来在元素属性值中插入一个JavaScript表达式。

在属性中嵌入JavaScript表达式时，不能够在大括号的外面加上引号，否则里面的JavaScript表达式将不生效，当做一个字符串字面量做处理。

<aside>
⭐ 由于JSX在语法上更接近JavaScript而不是HTML，所以ReactDOM使用小驼峰命名（camelCase）来定义属性的名称，而不使用HTML属性名称的命名制。

例如在JSX中，class变为className，tabindex变为tabIndex等。

</aside>

## JSX表示对象

若使用 Babel 编译器，Babel 会把 JSX 转移成一个名为 `React.createElement()` 函数调用。

代码转换示例（以下两种代码完全等效）：

```jsx
const element = (
	<h1 className="greeting">
		Hello, world!
	</h1>
);
```

```jsx
const element = React.createElement(
	'h1',
	{className: 'greating'},
	"Hello, world!"
);
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```jsx
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。