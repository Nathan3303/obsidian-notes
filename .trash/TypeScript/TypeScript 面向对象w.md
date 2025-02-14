## 类的基本定义

一个类中通常包含有多个属性(attribute) 和多个方法(function)，以及构造函数 constructor

```typescript
class className {
    /**
     * 定义实例属性
     * @brief 直接定义的属性是实例属性，需要通过对象的实例去访问 `(new Person()).name`
     * @caption 关键字 static 表示该属性为静态属性，能够直接通过类名访问到 `Person.age`
     * @caption 关键字 readonly 表示该属性为只读属性，因此不能重新赋值
     */
    attribute: type;
    /* ... */

    /**
     * constructor 构造函数
     * @brief 构造函数会在类被实例化（对象创建）时被调用
     * @caption
     *  - 在构造函数中的 this 的指向即为实例化后的新的对象，因此通过 this.[attribute]
     *  - 可以访问到实例化后对象的属性并进行修改
     */
    constructor(arg: type /* ... */) {
        this.attribute = arg;
        /* ... */
    }

    functionName() {
        /* Do something... */
    }
}
```

## 类的实例化（创建对象）

```javascript
const person = new Person(/* args...*/);
console.log(/* person.[attribute] */);
```

## 类的继承

使用继承后，子类将会拥有父类中的所有属性和方法。若多个类中拥有多个一致的属性或方法，那么可以将其
抽取到某个新类中，并使这个新的类成为这些类的父类。

```typescript
class Animal {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    sayHello() {
        console.log("Animal is saying hello!");
    }
}

class Dog extends Animal {
    constructor(name: string, age: number) {
        // super 函数用于调用父类的构造函数，并需要将固定的形参传入该函数
        super(name, age);
    }

    // 若子类中也定义了和父类一样的方法名，那么该方法会重写父类的同名方法。
    sayHello() {
        // 在类的方法中，我们可以通过 super 来访问父类
        super.sayHello(); // 调用父类的 sayHello 方法
        console.log("www!~");
    }
}

const dog = new Dog("wangcai", 3);
console.log(dog.name, dog.age); // wangcai 3
dog.sayHello(); // 输出 Animal is saying hello! 和 www!~

class Cat extends Animal {
    color: string | null;

    constructor(name: string, age: number, color?: string) {
        super(name, age);
        if (color) this.color = color;
    }
}

const cat = new Cat("mimi", 2, "black and white");
console.log(cat.name, cat.age, cat.color); // mimi 2 black and white
cat.sayHello(); // Animal is saying hello!
```

## 抽象类以及抽象方法

### 抽象类

通常某个父类仅仅只会被用来继承，很少会被实例化，因此我们需要增加一些限制，使得这些父类不能够被
实例化，这样的类被称为抽象类，通过关键字 abstract 修饰

```typescript
abstract class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    constructor(name: string, age: number) {
        super(name, age);
    }
}

class Teacher extends Person {
    constructor(name: string, age: number) {
        super(name, age);
    }
}

const person = new Person("test", 18); // Error: Cannot create an instance of an abstract class.
const student = new Student("crazyflame", 22);
const teacher = new Teacher("nathan", 34);
```

### 抽象方法

抽象方法通过关键字 abstract 指定，意为声明一个方法原型（方法原型的声明不需要指定方法体
，参考 C 语言中的函数原型定义）。在某个子类继承了父类时，若父类中含有抽象方法，子类必须重
写该方法，否则抛出错误

```typescript
abstract class Person {
    // 在 TypeScript 中需要为抽象方法定义返回值类型
    // 需要注意的是抽象方法必须在抽象类中才允许定义
    abstract sayHello(): void;
}

class Student extends Person {
    // 重写 sayHello 方法
    sayHello() {
        console.log("Hello! I am a student!");
    }

    readBook() {
        console.log("学生正在阅读书籍");
    }
}

class Teacher extends Person {
    // 重写 sayHello 方法
    sayHello() {
        console.log("Hello! I am a teacher!");
    }

    teachStudent() {
        console.log("老师正在教育学生");
    }
}

const student = new Student("crazyflame", 22);
const teacher = new Teacher("nathan", 34);
student.sayHello(); // Hello! I am a student!
teacher.sayHello(); // Hello! I am a teacher!
student.readBook(); // 学生正在阅读书籍
teacher.teachStudent(); // 老师正在教育学生
```

## 接口

接口用于描述一个类（对象）的结构，用来规定一个类中应该包含哪些属性和方法。通常是先有接口，再创建类，通过类实现该接口。接口能够重复定义，编译器会识别将多个重名接口中的属性或方法合并成一个。接口中的所有属性都不能有具体的值，所有方法都不能够指定方法体

```typescript
interface myInterface {
    name: string;
    age: number;

    sayHello(): void;
}
// 重复定义，使其合并
interface myInterface {
    gender: "male" | "female";

    talk(): void;
}

class Person implements myInterface {
    name: string;
    age: number;
    gender: "male" | "female";

    constructor(name: string, age: number, gender: "male" | "female") {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    sayHello() {
        console.log("Hello!");
    }

    talk() {
        console.log("I am very happy!");
    }
}

const person = new Person("nathan", 23, "male");
console.log(person.name, person.age, person.gender);
person.sayHello();
person.talk();
```

## 属性的修饰以及封装

### public 修饰符

被 public（公有属性） 修饰符所修饰的属性能够在任意位置被读取和修改

```typescript
class Person {
    // 默认不写即相当于 public
    name: string;
    public age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

const person = new Person("nathan", 20);
person.name = "crazyflame";
person.age = 22;
console.log(person.name, person.age); // crazyflame 22
```

### private 修饰符

被 private（私有属性） 修饰符所修饰的属性仅仅能够在类（实例）内部被读取和修改。那么此时我们需要
在类的内部添加对应的方法进而实现对这些私有属性的读取和修改，即 Getter 以及 Setter 。

```typescript
class Person {
    private name: string;
    private age: number;
    private _gender: "male" | "female";

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    getName = () => this.name;
    setName(newName: string) {
        this.name = newName;
    }

    getAge = () => this.age;
    setAge(newAge: number) {
        if (newAge >= 0 && newAge <= 110) this.age = newAge;
    }

    /**
     * Getter 和 Setter 的新写法
     */
    get gender() {
        return this._gender;
    }
    set gender(newGender: "male" | "female") {
        this._gender = newGender;
    }
}

const person = new Person("nathan", 20);
person.setName("crazyflame");
person.setAge(22);
person.setAge(200);
person.gender = "male"; // 新写法能够适配原始的等号赋值语法
console.log(person.getName(), person.getAge(), person.gender); // crazyflame 22 male
```

### protected 修饰符

被 protected 修饰符修饰的属性表示该属性是被保护的，其程度介于 public 以及 private 之间。被 protected
所修饰的属性只能在当前类以及子类中读取和修改。

### 关于构造函数的简化写法

由于大多数属性会在构造函数中被初始化，因此我们可以直接在构造函数的形参中直接声明需要的属性

```typescript
class Person {
    constructor(
        public name: string,
        private age: number,
        private gender: string
    ) {}
}

const person = new Person("John", 22, "male");
console.log(person); // Person { name: 'John', age: 22, gender: 'male' }
```