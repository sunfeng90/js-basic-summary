# js-basic-summary
整理JavaScript的基础且重要的知识
### 数据类型
  - 原始类型:Undefined、Null、Boolean、Number、String、Symbol
  - 引用类型: Object(Array, Object, Array, Date, Function)
#### typeof
- 作用：用于检查数据的类型
- 注意：
```
typeof null //在某些场景下会返回null，也会返回Object
typeof RegExp // 在某些浏览器会返回function，在某些浏览器会返回object
```
#### Undefined
- 值：undefined
- 出现场景：声明了变量，但是未赋值，此时变量的值就是undefined
- 注意事项：
  - 不需要显示的声明变量为undefined
  - 在非严格模式下，对未初始化和未声明的变量执行typeof操作，都会返回undefined。在严格模式下，会直接报错
  - 在非严格模式下，能初始化变量的时候，尽量去初始化变量。这样指向typeof就不会造成歧义
```
var message;
console.log(typeof message); // 输出undefined
console.log(typeof age); // 输出undefined
```
#### Null
- 值：null
- 注意事项：
  - 如果某个变量，未来要存对象，那么直接初始化赋值为null，这样判断是否存对象就直接拿null进行对比
  - 在非全等的情况下，null和undefined是相等的。在全等的情况下，null和undefined是不等的。
```
var obj = null;
console.log(typeof obj); // 输出object
console.log(null == undefined) // true
console.log(null === undefined) // false
```
#### Boolean
- 值：true/false
- 注意：
   - true和1，false和0是不等的
   - 区分大小的。True和False都不是Boolean的值，只是标识符
#### Number
- 格式：IEEE754格式
- 类型：十进制、八进制、十六进制
- 范围：在大多数浏览器为5*10^-324 ~ 1.8*10^308
- NaN: 非数值
  - 特点：
    - 任何涉及NaN的操作，都会返回NaN
    - NaN与任何数值都不等，包括它自己
- 数值转换：parseInt、parseFloat、Number
- 注意：
  -  不建议浮点数前面的整数省略；
  -  浮点数需要的内存空间是整数的两倍；
  -  浮点数最高精度为17位，在进行算术计算时精度远远不如整数；
  -  之所以进行算术运算会造成舍入误差，是因为采用IEEE754标准造成的，是IEEE754的通病
```
console.log(0.15 + 0.25 === 0.4); // true
console.log(0.15 + 0.15 === 0.3); // true
console.log(0.1 + 0.2 === 0.3) // false
```
#### String
- 表示法：单引号和双引号。单或者双引号表示的效果都一样。
- 转换：String(123)、"" + 1、toString()
#### Symbol
#### Object
- 表示法：
```
// 下面两个表示法等价
new Object()
new Object
```
### 重载
- 没有重载：因为函数的参数是由包含零或多个值的数组来表示的。没有函数签名签名，真正的重载是不可能做到的。
- 如何实现重载：通过检查传入函数的类型和数量并作出不同的反应，从而模仿方法的重载。
- 如果声明多个同名函数，那么后面声明的会覆盖前面声明的函数
### 变量和作用域
- 如何判断一个类型：
  - 基本数据类型：typeof
  - 引用数据类型：instanceof
  - 注意：
    - 如果用instanceof检查基本数据类型，始终会返回false，因为基本类型不是对象
    - 所有引用类型都是Object，所以用instanceof检查各种引用类型，始终返回true
### 垃圾收集策略
- 标记清除：常见
- 引用计数：引用的次数为0时，就清除该变量。如果相互引用或者被某个函数重复多次调用的时候，就导致很多变量无法被回收。
### Array
- isArray(): 检查是否是数组
- toString(): 转换成数组
- push、pop、shift、unshift
- sort、reverse
- slice、concat、splice
- indexOf、lastIndexOf
- every、filter、map、forEach、some
- reduce、reduceRight
### 函数表达式
- 函数声明提升:在执行代码之前会先读取函数声明，可以把函数声明放在调用它的后面；
  ```
   sayHi(); 
   function sayHi() {
     console.log('Hello World');
   }
  ```
- 创建函数的方式是使用函数表达式；
- 函数表达式必须先赋值才能使用；
### 闭包
- 定义：有权访问另外一个函数作用域中变量的函数。
### 面向对象
- 如何创建对象？
```
// 法1
// const person = new Object();
// person.name = "Frank";
// person.age = 29;
// person.job = "Software Engineer";
// person.sayName = function() {
//   console.log(this.name);
// }

// 法2
// const person = {
//   name: 'Frank',
//   age: 30,
//   job: "Software Engineer",
//   sayName: function() {
//     console.log(this.name);
//   }
// }
```
- 属性类型
   - 数据属性
     - Configureable
     - Enumerable
     - Writable
     - Value
```
const person = {};

Object.defineProperty(person, "name", {
  writable: false,
  value: "Gold"
});
console.log(person.name);
person.name = "Frank";
console.log(person.name);
```
   - 访问器属性
     -  Configurable：默认值为true
     -  Enumerable：默认值为true
     -  Get：默认值为undefined
     -  Set: 默认值为undefined
```
const book = {
  _year: 1,
  edition: 1
};

Object.defineProperty(book, "year", {
  get: function() {
    return this._year;
  },
  set: function(newValue) {
    if (newValue > 2004) {
      this._year = newValue;
      this.edition += newValue - 2004;
    }
  }
});
book.year = 2005;
console.log(book.edition);
```
- 创建对象使用到的模式
   - 工厂模式
     - 定义：通过一个函数封装创建对象的细节，类似工厂。
     - 缺点：无法识别多个不同的对象。
     - 例子：
```
function createPerson(name, age, job) {
  const o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function() {
    console.log(this.name);
  };
  return o;
}
const person1 = createPerson("Frank", 30, "Software Engineer");
const person2 = createPerson("Greg", 30, "Doctor");
person1.sayName();
person2.sayName();
```
   - 构造函数模式
     - 缺点：每个方法都要在每个实例上重新创建一遍。
     - 例子：
```
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() {
    console.log(this.name);
  };
};
// 当作构造函数使用
// const person1 = new Person("Frank", 30, "Software Engineer");
// const person2 = new Person("Greg", 30, "Doctor");

// 作为普通函数调用
Person("Greg", 30, "Doctor");
window.sayName();

// 在另外一个对象的作用域中调用
const obj = new Object();
Person.call(obj, "Frank", 30, "Nurse");
obj.sayName();
```
   - 原型模式
     - 含义：每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象。而这个对象的用途
     是包含可以由特定类型的所有实例共享的属性和方法。
     - 好处：可以让所有对象实例共享它所包含的属性和方法。
     - 其他：
       - prototype
         在脚本中没有标准的方式访问Prototype,但Firefox、Safari和Chrome在每个对象上都支持一个属性__proto__。__proto__存在于实例和构造函数的原型对象之间，而不是存在于实例与构造函数之间。
     - 注意：实例中的指针仅指向原型，而不指向构造函数。
     - 缺点：
       - 省略了为构造函数传递初始化参数这一环节，这样将导致所有实例在默认情况下都将取得相同的属性值
       - 很多属性都被多个实例共享
     - 例子：
```
function Person() {
}
Person.prototype.name = "Frank";
Person.prototype.age = 30;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function() {
  console.log(this.name);
}
const person = new Person();
person.sayName();
     
// function createPerson(name, age, job) {
//   const o = new Object();
//   o.name = name;
//   o.age = age;
//   o.job = job;
//   o.sayName = function() {
//     console.log(this.name);
//   };
//   return o;
// }
// const person1 = createPerson("Frank", 30, "Software Engineer");
// const person2 = createPerson("Greg", 30, "Doctor");
// person1.sayName();
// person2.sayName();

// function Person(name, age, job) {
//   this.name = name;
//   this.age = age;
//   this.job = job;
//   this.sayName = function() {
//     console.log(this.name);
//   };
// };
// 当作构造函数使用
// const person1 = new Person("Frank", 30, "Software Engineer");
// const person2 = new Person("Greg", 30, "Doctor");

// 作为普通函数调用
// Person("Greg", 30, "Doctor");
// window.sayName();

// 在另外一个对象的作用域中调用
// const obj = new Object();
// Person.call(obj, "Frank", 30, "Nurse");
// obj.sayName();

function Person() {
}
Person.prototype.name = "Frank";
Person.prototype.age = 30;
Person.prototype.job = "Software Engineer";
Person.prototype.friends = ["Kobe", "James"];
Person.prototype.sayName = function() {
  console.log(this.name);
}
const person = new Person();
person.sayName();
console.log(Person.prototype.isPrototypeOf(person)); // 返回true
console.log(Object.getPrototypeOf(person) == Person.prototype); // true
console.log(Object.getPrototypeOf(person).name); // Frank

console.log(person.hasOwnProperty("name")); // false

const person2 = new Person();
person2.name = "Greg";
console.log("name" in person); // true
console.log("name" in person2); // true

console.log(person2.name); // Greg
console.log(person.name); // Frank

console.log(person2.hasOwnProperty("name")); // true
console.log("name" in person2); // true

delete person2.name;
console.log(person2.name); // Frank
console.log("name" in person2); // true
console.log(person2.hasOwnProperty("name")); // false

String.prototype.startWith = function(text) {
  return this.indexOf(text) === 0;
};
const msg = "HelloWorld!";
console.log(msg.startsWith("Hello")) // 返回true

person2.friends.push("Frank");
console.log(person.friends); // [ 'Kobe', 'James', 'Frank' ]
console.log(person2.friends); // [ 'Kobe', 'James', 'Frank' ]
```
  - 组合使用构造函数和原型模式
     - 好处：构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。
     - 例子：
```
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ["Kobe", "James"];
}
Person.prototype = {
  constructor: Person,
  sayName: function() {
    console.log(this.name);
  }
}
const person1 = new Person("Frank", 30, "Software Engineer");
const person2 = new Person("Gold", 29, "Doctor");
person1.friends.push("Van");

console.log(person1.friends); // [ 'Kobe', 'James', 'Van' ]
console.log(person2.friends); // [ 'Kobe', 'James' ]
console.log(person1.friends === person2.friends); // false
console.log(person1.sayName === person2.sayName); // true
```
   - 动态原型模式
     - 好处：通过在构造函数中初始化原型，又保持了同时使用构造函数和原型的优点
     - 例子：
```
function Person(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
  if (typeof this.sayName !== 'function') {
    Person.prototype.sayName = function() {
      console.log(this.name);
    }
  }
}
const gold = new Person("Gold", 30, "Software Engineer");
gold.sayName(); // Gold
```
   - 寄生构造函数模式
     - 注意：返回的对象与构造函数或者与构造函数的原型属性之间没有关系；构造函数返回的对象与在构造函数外部创建的对象没有什么不同 
     - 例子：
```
function SpecialArray() {
  const values = new Array();
  values.push.apply(values, arguments);

  values.toPipedString = function() {
    return this.join("|");
  };

  return values;
}
const colors = new SpecialArray("red", "blue", "green");
console.log(colors.toPipedString());
```
   - 稳妥构造函数模式
     - 定义：所谓稳妥对象，是指没有公共属性，而且其他方法也不引用this的对象。稳妥对象最适合在一些安全的环境中或者防止数据被其他应用程序改动时使用。
```
function Person(name, age, job) {
  const o = new Object();
  o.sayName = function() {
    console.log(name);
  };
  return o;
}
const friend = Person("Frank", 30, "Software Engineer");
friend.sayName(); // Frank
- 继承