# 特点

1. Symbol 的值是唯一的，用来解决命名冲突的问题。
2. Symbol 不能与其他数据进行运算，如加减乘除、字符串拼接、比较。
3. Symbol 定义的对象属性不能使用 for…in 循环遍历，但是可以使用 Reflect.ownKeys 来获取对象的所有键名。

# 创建

创建 Symbol 通过函数 `Symbol()` 创建。

```jsx
let s = Symbol();
console.log(s, typeof s);    // Symbol() "symbol"

let s2 = Symbol('test');   // 其中传入的字符串 test 为该 symbol 的标识
let s3 = Symbol('test');
//# Symbol 函数会返回一个独一无二的值，而其中传入的参数则是这个独一无二的值的一个描述（标识）
console.log(s2 === s3);    // false

let s4 = Symbol.for('test');
let s5 = Symbol.for('test');
//# Symbol.for 函数根据传入的参数返回一个独一无二的值，因此在多次调用时传入的参数相同，
//# 则返回的值相同。
console.log(s4 === s5);    // true
```

# 使用

Symbol 可以用于向对象中添加一个独一无二的方法。

```jsx
let test = {
	name:"test",
	[Symbol('a')]: function () {
		console.log("a");
	},
	[Symbol('b')]: function () {
		console.log("b");
	}
}

console.log(test);    // { name: "test", Symbol(a): *f*, Symbol(b): *f* }
```

# 内置属性

### **`Symbol.hasInstance`**

**`Symbol.hasInstance`** 用于判断某对象是否为某构造器的实例。因此你可以用它自定义 `instanceof` 操作符在某个类上的行为。

```jsx
class MyArray {
	static [Symbol.hasInstance](instance) {
		/**
				1. 在函数中可以通过一些检测手段来判断传入的实参是否满足要求。
				2. 该函数只能通过 instanceof 来触发，instanceof 关键字前面的值作为该函数的实参。
		 */
		return Array.isArray(instance);
	}
}
console.log([] instanceof MyArray);    // true
```

### `Symbol.isConcatSpreadable`

内置的 **`Symbol.isConcatSpreadable`** 符号用于配置某对象作为 `Array.prototype.concat()` 方法的参数时是否展开其数组元素。

```jsx
let a = [1, 2, 3];
let b = [4, 5, 6];

b[Symbol.isConcatSpreadable] = false;
let ab = a.concat(b);
console.log(ab);    // [1, 2, 3, [4, 5, 6]]

b[Symbol.isConcatSpreadable] = true;
let ab = a.concat(b);
console.log(ab);    // [1, 2, 3, 4, 5, 6]
```

# JavaScript 的七种数据类型

JavaScript 七种数据类型简记：`USONB`

1. u = undefined
2. s = string  /  symbol
3. o = object
4. n = null  /  number
5. b = boolean