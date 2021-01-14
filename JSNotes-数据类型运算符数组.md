## 1 数据类型
### 1.1 初识JS
1. 什么是JS?
JS是用于网页开发实现网页动态交互的高级脚本语言，原名Livescript（后面为了蹭Java热度，改名为JavaScript)，由Netscape公司发明，后交由Mozilla基金会(MDN)维护。

JS的标准是ECMAScript,目前普遍使用的版本是ECMAScript5.1(ES5),最新版本是ES6（2015年发布）

2. JS的执行原理
由浏览器的JS引擎逐行读取JS代码解释执行，读取一行执行一行

3. JS的三大组成部分
- ECMAScript
- 文档对象模型DOM
- 浏览器对象模型BOM

4. JS的输入输出语句
- 弹窗式输出：`alert('hello')`
- 控制台打印式：`console.log('hello')`
- 弹窗式输入：`prompt('提示文本')`

### 1.2 变量
1. 变量的主要作用
用于存储数据

2. 变量的初始化
```javascript
var v1 = 1;
let v2 = 'a';
```
var和let的区别：
var拥有函数作用域，let是后出现的，拥有块级作用域，相比var使用更规范（如var可以重复声明变量但let不行），条件允许建议使用let

3. 变量的命名规范
和C/C++/Java相比多了一个$，即由数字、字母、下划线和$组成且开头不能是数字

4. 变量在内存中如何存储的？
声明变量后会在内存中开辟一段空间并将变量名指向该内存空间

### 1.3 数据类型导读
1. 5种简单的数据类型
- number数字型
- string字符串型
- boolean布尔型
- undefined
- null
2. 如何获取变量类型
使用`typeof 变量名`
3. 字符串转换为数值型的方法
- parseInt()和parseFloat()
- 使用Number()强制类型转换
- 利用算术运算- * /进行隐式转换（JS引擎根据上下文推断变量的数据类型，不能直接运算时会尝试进行自动转换） `console.log(‘12’-0)`

4. 转换为字符串型的方法
- 使用+和空字符串拼接 `console.log(1+"")`
- toString()方法       

## 2 运算符
### 2.1 常用运算符
1. 算术运算符 +，-，*，/
2. 关系运算符 >,<,>=,<=
3. 相等运算符 \==,!=,\===,!\==（后两个不仅值要相等，类型也要相等）
3. 逻辑运算符 &&,||,!
5. 赋值运算符

**补充** 逻辑中断（短路）运算符（使用非布尔值参与逻辑运算）
短路运算的原理：当有多个表达式（值）参与运算时，如果左边的表达式（值）可以确定，就不再继续运算右边的表达式的值
1. 逻辑中断逻辑与： `表达式1&&表达式2`
- 如果第一个表达式的值为真，返回表达式2
- 如果第一个表达式的值为假，返回表达式1
2. 逻辑中断逻辑或： `表达式1||表达式2`
规则与逻辑与相反
注意：表达式的值为空的或否定的为假，如`0,null,false,NaN,undefined`
### 2.2 前置递增和后置递增的区别

### 2.3 运算符优先级
大体逻辑：
- 单目(!,++,--,etc)>双目
- 算术>关系>逻辑>赋值>逗号

## 3 流程控制
### 3.1 if分支语句
```
if(判断条件) {

}else if(){

}else{

}
```
### 3.2 switch分支
```
switch(分支变量) {
  case 分支变量值1：
    ...
    break;
  case 分支变量值2：
    ...
    break;
  default:
    ...
    break;
}
```
**switch和ifelseif区别:**
- switch通常处理case为比较确定值的情况；ifelseif语句适合范围判断
- switch语句条件判断后直接执行相应语句，执行效率更高一些
- switch更适合分支多的情况，ifelseif更适合分支少的情况

### 3.3 循环
1. 循环的目的
- 重复执行某些操作
2. for循环
```
for(初值条件;循环终止条件;循环递增){
  循环体;
}
```
3. while循环和do while循环
- while循环：先判断后执行
- do while循环：先执行后判断
4. break和continue区别
- break:终止跳出循环
- continue:结束当前循环进入下次循环

## 4 数组
### 4.1 为什么要有数组
可以用来存储一系列不同的元素，类似python中的list
### 4.2 如何创建数组
```
var arr = []; // 空数组
arr = [1, 'a']; 
```
### 4.3 如何获取数组中的元素
利用下标访问

### 4.4 如何对数组遍历
```javascript
// 写法1
arr = [1, 'a', true];
for(let k in arr) {
  console.log("key:"+ k + ",value:" + arr[k]);
}
// 写法2
arr.forEach((item, idx, arr) => console.log("key:"+ idx + ",value:" + item));
```
### 4.5 数组转字符串
- `arr.join(分隔符)`
- `arr.toString()`
### 4.5 给数组新增元素
- 尾部新增：`arr.push()`
- 头部新增：`arr.unshift()`
### 4.6 删除数组元素
- 尾部删除：`arr.pop()`
- 头部删除：`arr.shift()`
### 4.7 冒泡排序
```javascript
let arr = [3, 4, 1, 2];
for(let i=0; i<arr.length-1; i++) {
  for(let j=0; j<arr.length-1-i; j++) {
    if(arr[j] > arr[j+1]) {
      let tmp = arr[j]; 
      arr[j] = arr[j+1];
      arr[j+1] = tmp;
    }
  }
}
console.log(arr)
```