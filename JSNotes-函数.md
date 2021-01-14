
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [1 函数](#1-函数)
  - [1.1 JS参数传递形式](#11-js参数传递形式)
  - [1.2 函数形参与实参个数不匹配问题](#12-函数形参与实参个数不匹配问题)
  - [1.3 函数返回值](#13-函数返回值)
  - [1.4 arguments的使用](#14-arguments的使用)
  - [1.5 函数的两种声明方式](#15-函数的两种声明方式)
- [2 JS的作用域和作用域链](#2-JS的作用域和作用域链)
  - [2.1 什么是JS的作用域](#21-什么是JS的作用域)
  - [2.2 JS作用域划分](#22-JS作用域划分)
  - [2.3 作用域链](#23-作用域链)
- [3 JS的预解析](#3-JS的预解析)
<!-- /code_chunk_output -->

## 1 函数
### 1.1 JS参数传递形式
简单数据类型按照值传递
```js
function testArgs(argu) {
    argu++;
    return argu;
}

argu = 1;
console.log(testArgs(argu)); // 2
console.log(argu); // 1
```
复杂数据类型按照地址传递 
```js
function Person(name) {
  this.name = name;
}

function f1(x){
  x.name = "Mike";
  console.log(x.name);
}

var p = new Person("Blate");
console.log(p.name); // "Blate"
f1(p); // p将其保存的地址传给了形参x
console.log(p.name); // "Mike"
```
### 1.2 函数形参与实参个数不匹配问题
- 实参个数多于形参，只取到形参个数
- 实参个数少于形参，没有接受到实参的形参（可以看作是不用声明的变量）是undefined
e.g.
```js
function getSum(num1, num2) {
  return num1 + num2;
}
getSum(1); // 1 + undefined -> NaN
```
### 1.3 函数返回值
- 函数有return，返回return后面的值
- 函数没有return，返回的是undefined

### 1.4 arguments的使用
arguments是每个函数内置的一个对象，里面按照伪数组的形式存储了所有传递过来的实参
ps: 伪数组拥有数组的一些属性，如length，能够通过下标访问，但是不具备数组方法

### 1.5 函数的两种声明方式
1. 利用函数关键字自定义函数（命名函数）
2. 函数表达式声明（匿名函数）
```js
 var fun = function(name) {console.log(name)};
 fun("ygj"); // 调用函数
 ```
- fun是变量名，不是函数名
- 匿名函数和声明变量差不多，只不过变量里面存的是函数
- 函数表达式可以进行参数传递


## 2 JS的作用域和作用域链
### 2.1 什么是JS的作用域？
代码名字（变量）起作用和效果的范围，目的是为了提高程序的可靠性，更重要的是**减少命名冲突**

### 2.2 JS作用域划分
ES6之前：全局作用域和局部作用域
- **全局作用域**：整个script标签内或者是一个单独的js文件内，是全局变量的作用域
- **局部作用域（函数作用域）**:函数内部，是局部变量（在函数内声明的变量）的作用域
注意：在函数内未声明但是赋值的变量是全局变量，可以在函数外使用

ES6中添加了块级作用域，即{}作用域，如if(){}, for(){}

### 2.3 作用域链
1. 概念：内部函数访问外部函数的变量，采取的是由内而外的链式查找的方式来决定取哪个值，这种结构称为作用域链，即**就近原则**
2. 分析方法：站在目标出发，一层一层往外找（必要时可画图辅助分析）
```js
function f1() {
  var num = 123;

  function f2() {
    var num = 0;
    console.log(num);
  }

  f2();
}

var num = 456;
f1(); // 输出结果是0
```

## 3 JS的预解析 
### 3.1 js引擎运行js分两步——预解析和代码执行
1. 预解析 ：js引擎会把js里面所有的var和function提升到当前作用域的最前面（变量放在更前面）
2. 代码执行：按照代码书写的顺序从上往下执行
### 3.2 预解析分为变量预解析（变量提升）和函数预解析（函数提升）
1. **变量提升**：把所有变量声明提升到当前作用域最前面（不提升赋值操作）

e.g.1
直接运行`console.log(num)`会报错，但运行下面的代码则会报undefined
```js
console.log(num); 
var num = 10;
```
原因就是js引擎按照变量预解析把上面代码转换成了下面的实际代码
```js
var num;
console.log(num);
num = 10;
```
e.g.2
运行
```js
var fun = function() {console.log("hello")};
fun();
```
正常，但运行
```js
fun();
var fun = function() {console.log("hello")};
```
则会报错函数未定义，原因也是预解析的问题。
js引擎按照变量预解析把上面代码转换成了下面的实际代码：
```js
var fun;
fun();
fun = function() {console.log("hello")};
```
2. **函数提升**：把关键字声明的函数声明提升到当前作用域最前面（不调用函数）
e.g.3
如下代码可以正常执行就是js引擎执行了函数提升
```js
fun();
function fun() {console.log("hello")};
```

### 3.3 预解析和作用域链综合案例
案例1
```js
var num = 10;
function fn() {
  console.log(num);
  var num = 20;
  console.log(num);
}
fn();
```
==>
```js
var num;
function fn() {
  var num;
  console.log(num); // 就近原则，undefined
  num = 20;
  console.log(num); // 20
}
num = 10;
fn();
```
案例2
```js
f1();
console.log(c);
console.log(b);
console.log(a);

function f1(){
  var a = b = c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
```
==>
```js
function f1(){
  // var a = b = c = 9;相当于 var a = 9; b = 9; c = 9; b,c直接赋值没有var声明，相当于全局变量
  var a;
  a = 9;
  b = 9;
  c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
f1();
console.log(c);
console.log(b);
console.log(a); // 报错
```

