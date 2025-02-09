# 声明常量

通过关键字 `const` 定义常量。常量在定义后不能够被修改。

```jsx
const a = 123;
const b = [];
const c = {};
const d = "123";
```

# 关键字特性

## 定义后无法被修改

由于常量在定义后不能被修改，因此规定在常量定义时一定要赋予初始值，否则编译不通过。

```jsx
const a; // Error
const a = 123;
```

## 拥有块级作用域特性

不同作用域中的同名常量之间不会相互影响。

```jsx
const a2 = 123;

{
    const a2 = 456;
    console.log(a2); // 456
}

console.log(a2); // 123
```

## 对象常量

对象或数组声明为常量时，其元素或属性可以被修改。这是因为常量实际保存的是对象或数组的引用，而非其内容。

```jsx
const obj = { name: 'John' };
obj.name = 'Jane';
```