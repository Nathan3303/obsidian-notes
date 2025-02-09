# ES5 的获取方式（arguments）

```jsx
function test() {
	console.log(arguments);
}
test('a', 'b', 'c');    // 是一个以数组形式保存的对象
```

# ES6 的获取方式

```jsx
function test(...args) {  // 通过 ...args 的方式获取
	console.log(args)
}
test('a', 'b', 'c');    // ['a', 'b', 'c']    ## 是一个数组
```