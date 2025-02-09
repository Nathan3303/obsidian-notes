组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

组件的定义分为两种：

- 通过函数形式定义的组件，称为函数式组件。
- 通过类（class)形式定义的组件，称为类（class）组件。

下面将展示两者的定义形式。

## 函数式组件与类式组件

### 函数式组件

定义组件最简单的方式就是编写 JavaScript 函数：

```jsx
function Welcome(props) {
	return (
		<div>
			<h1>Hello,{props.name}</h1>
		</div>
	);
}
```

该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

### 类式组件

我们还可以使用ES6中的class来定义组件：

```jsx
class Welcom extends React.Component {
	render() {
		return (
			<div>
				<h1>Hello,{props.name}</h1>
			</div>
		);
	}
}
```

要注意的是类式组件必须继承React.Component这个类。该类式组件与上述的函数组件在React里是等效的。

函数式组件和类式组件的具体特性将在后面的章节中讨论。

## 渲染组件

在接触组件之前，我们渲染的都只是DOM标签，但在接触组件之后，我们可以发现，组件最终返回的也是DOM标签。那我们应该如何去使用组件？

使用组件的方式类似于使用DOM标签：

```jsx
const element = <Welcome name="Nathan" />;
```

<aside>
💡 这里我们需要注意的是，组件名称的首字母必须大写，以避免与其他元素标签冲突。

React 会将小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 `Welcome`。

</aside>

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “props”。

例如上述组件示例，name 是要传入组件的某个属性，且值为 Nathan，那么在 Props 对象中则有成员`name:"Nathan"` 。在组件中通过 `props.name` 即可获取到传入的值。

例如下面这段代码，这段代码会在页面上渲染出“Hello, Nathan”。

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Nathan" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

## 组合组件

组件可以在其输出中引用其他组件。这就可以让我们用同一组件来抽象出任意层次的细节。按钮，表单，对话框，甚至整个屏幕的内容：在 React 应用程序中，这些通常都会以组件的形式表示。

例如，我们可以创建一个可以多次渲染 `Welcome` 组件的 `App` 组件：

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
	return (
		<div>
			<Welcome name="Sara" />
			<Welcome name="Cahal" />
			<Welcome name="Edite" />
		</div>
	);
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

通常来说，每个新的 React 应用程序的顶层组件都是 `App` 组件。但是，如果你将 React 集成到现有的应用程序中，你可能需要使用像 `Button` 这样的小组件，并自下而上地将这类组件逐步应用到视图层的每一处。

## 提取组件

提取组件，顾名思义，就是将组件拆分为更小的组件。

例如，参考如下的Comment组件：

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

该组件用于描述一个社交媒体网站上的评论功能，它接收 `author`（对象），`text` （字符串）以及 `date`（日期）作为 props。

该组件由于嵌套的关系，变得难以维护，且很难复用它的各个部分。因此，让我们从中提取一些组件出来。

首先，我们将提取 `Avatar` 组件：

```jsx
function Avatar(props) {
	return (
		<img className="Avatar"
					src={props.user.avatarUrl}
					alt={props.user.name}
		/>
	);
}
```

`Avatar` 不需知道它在 `Comment` 组件内部是如何渲染的。因此，我们给它的 props 起了一个更通用的名字：`user`，而不是 `author`。

我们建议从组件自身的角度命名 props，而不是依赖于调用组件的上下文命名。

我们现在针对 `Comment` 做些微小调整：

```jsx
function Comment(props) {
	return (
			<div className="Comment">
				<div className="UserInfo">
					<Avatar user={props.author} />
				<div className="UserInfo-name">
					{props.author.name}
				</div>
			</div>
			<div className="Comment-text">
			{props.text}
			</div>
			<div className="Comment-date">
			{formatDate(props.date)}
			</div>
		</div>
	);
}
```

接下来，我们将提取 `UserInfo` 组件，该组件在用户名旁渲染 `Avatar` 组件：

```jsx
function UserInfo(props) {
  return (
		<div className="UserInfo">
			<Avatar user={props.user} />
			<div className="UserInfo-name">
				{props.user.name}
			</div>
		</div>
	);
}
```

进一步简化 `Comment` 组件：

```jsx
function Comment(props) {
	return (
		<div className="Comment">
			<UserInfo user={props.author} />
			<div className="Comment-text">
				{props.text}
			</div>
			<div className="Comment-date">
				{formatDate(props.date)}
			</div>
		</div>
	);
}
```

最初看上去，提取组件可能是一件繁重的工作，但是，在大型应用中，构建可复用组件库是完全值得的。根据经验来看，如果 UI 中有一部分被多次使用（`Button`，`Panel`，`Avatar`），或者组件本身就足够复杂（`App`，`FeedStory`，`Comment`），那么它就是一个可复用组件的候选项。