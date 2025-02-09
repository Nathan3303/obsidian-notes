# 原生就具备 Iterator 接口的数据类型

<aside>
📌 在 ES6 中提供了一种新的遍历方式：for…of 循环，Iterator 的接口主要就是供 for…of 使用。

</aside>

1. Array
2. Arguments
3. Set
4. Map
5. String
6. TypedArray
7. NodeList

# 迭代器的使用示例

```jsx
const letters = ['a', 'b', 'c', 'd'];    //# 数组自带迭代器接口

for (let v of letters)   //# 通过 for...of 使用迭代器，获取键值
	console.log(v)    // a  b  c  d

for (let v in letters)   //# 通过 for...in 使用迭代器，获取键名
	console.log(v)    // 0  1  2  3

console.log(letters);    
//# 在其原型上存在 Symbol(Symbol.iterator): *f* values() 这个方法
//# 即迭代器接口
```

# 迭代器工作原理

1. 迭代器会创建一个指针对象，指向当前数据结构的起始位置；
2. 第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员；
3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员；
4. 每次调用 next 方法返回一个包含 value 和 done 属性的对象。

```jsx
const letters = ['a', 'b', 'c', 'd'];
let iteratorObj = letters[Symbol.iterator]();

// 调用 next 方法
console.log(iteratorObj.next());    // *{value: "a", done: false}*
console.log(iteratorObj.next());    // *{value: "b", done: false}*
console.log(iteratorObj.next());    // *{value: "c", done: false}*
console.log(iteratorObj.next());    // *{value: "d", done: false}*
console.log(iteratorObj.next());    // *{value: undefined, done: true}*
```

# 自定义遍历数据实现

```jsx
// 定义对象
const banji = {
    name: "class-a",
    students: ["xiaoming", "xiaoning", "xiaozhang", "xiaotian"],
    [Symbol.iterator]() {
        let index = 0;
        return {
            next: () => {
                if (index < this.students.length)
                    return {value: this.students[index++], done: false};
                else
                    return {value: undefined, done: true};
            },
        };
    },
};

// 遍历对象
for (const v of banji) {
    console.log(v);
}
```