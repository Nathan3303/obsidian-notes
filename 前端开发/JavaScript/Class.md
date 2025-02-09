在 ES6 中提供了更接近传统语言的写法，引入了 Class 这个改变，作为对象的模板。通过 Class 关键词可以定义一个类。

基本上，ES6 的 class 可以看作是一个语法糖，它的绝大部分功能在 ES5 中都可以实现，如构造函数。新的 class 的写法只是让对象原型更加清晰、更像面向对象的语法。

# 在 ES5 中实例化对象的写法

通过构造函数的方式进行实例化。

```jsx
// 定义一个构造函数
function test(a, b) {
		this.a = a;
		this.b = b;
}

// 在其原型上添加一个 call 方法
test.prototype.call = function () {
		console.log("test");
}

// 实例化
let testObj = new test('A', 'B');
testObj.call();
console.log(testObj);
```

# Class (类) 的相关知识点

1. 通过关键字 class 声明一个类
2. 通过 constructor 关键字定义构造函数初始化
3. 通过 extends 关键字继承父类
4. 通过 super 关键字调用父级构造方法
5. 通过 static 关键字定义静态方法和属性
6. 重写父类的方法

# 通过 class 关键字声明一个类

```jsx
// 定义一个类
class test{
		// 通过 constructor 关键字定义构造函数初始化
		constructor(a, b){
				this.a = a;
				this.b = b;
		}

		// 定义一个方法
		call(){
				console.log("test");
		}
}

// 实例化
let testObj = new test("A", "B");
testObj.call();
console.log(testObj);
```

# 类的静态成员 (static 关键字)

类中的 static 关键字用于定义类的静态属性，所谓静态属性即只存在于所定义的类身上，不存在于根据该类实例化出来的对象身上。

```jsx
class test{
		static name = "abc";
}

let testObj = new test();
console.log(testObj.name);    // undefined
console.log(test.name);       // abc
```

# 类的继承 (extends 关键字)

```jsx
// 父类
class Phone {
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }

    call() {
        console.log("我可以打电话!!");
    }
}

// 子类
class SmartPhone extends Phone {
    constructor(brand, price, color, size) {
        super(brand, price);    // 用于向父类构造器传递参数
        this.color = color;
        this.size = size;
    }

    playGame() {
        console.log("我可以打游戏");
    }
}

// 初始化子类
let smartPhoneObj = new SmartPhone("xiaomi", 3299, "red", 5.5);
console.log(smartPhoneObj);  // Object { brand: "xiaomi", price: 3299, color: "red", size: 5.5 }
smartPhoneObj.call();        // 我可以打电话!!
smartPhoneObj.playGame();    // 我可以打游戏
```

# 类中的 Getter 和 Setter

类中的 get 和 set 关键字可以将属性和一个方法进行绑定，此方法适用于动态属性的获取和修改。

比如对象中的一个属性通过 get 与一个方法进行绑定，则获取该属性时会自动执行所绑定的方法，通过一系列计算，此方法的返回值就是该属性的值，因此称为动态属性。

通过 set 关键字绑定的方法必须设置一个形参用于接收新的值。

```jsx
class test {
		get name() {
				return "test";
		}

		set name(newName) {
				console.log("changed");
		}
}

let testObj = new test();
console.log(testObj.name);    // test
testObj.name = "test2";       // changed
```