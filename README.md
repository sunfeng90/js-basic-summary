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