## 1 JS对象基础
### 1.1 何为对象以及为何需要对象？
- 对象是一个**具体的事物**，由属性和方法构成。女朋友不是对象，志玲姐姐才是对象(可惜志玲姐姐已经有对象了T_T)
![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1324736328,1878123183&fm=26&gp=0.jpg)
- 使用对象来表示事物结构更清晰,其本质是一组无序的相关属性和对象集合


### 1.2 创建对象的三种方法
#### 1.2.1 利用对象字面量创建对象
```javascript
// 创建对象
var dateObj = {
  name: "ygj",
  age: 26,
  sayHi: function() {
    alert("hi")
  }
}
// 使用对象
// 1. 对象.属性
dateObj.name;
// 2. 对象["属性"]
dateObj["name"]
// 3. 调用方法
dateObj.sayHi();
```

**补充**：变量、属性、函数、方法的比较
变量：单独声明赋值，单独存在
属性：对象中的变量，不需要声明，用来描述对象的特征，需要使用`对象.属性`或`对象["属性"]`访问
函数：单独存在，通过`函数名()`调用
方法：对象中的函数，用来描述对象的行为，使用`对象.方法()`调用

#### 1.2.2 利用new Object()创建对象
```js
var dateObj = new Object();
dateObj.name = 'ygj';
dateObj.age = 28;
dateObj.sayHi = function() {alert("hi");};
```

#### 1.2.3 利用构造函数创建对象
1. 为何需要这种创建方式？
前两种创建方式一次只能创建一个对象，而有时创建对象的方式相似，为避免重复书写，借用函数的思路编写专门创建对象的函数。
构造函数类似java/c++里面的类，泛指某一大类
2. 构造函数的语法
```js
// 声明
function 构造函数名(args...) { // 构造函数名需首字母大写
  this.属性 = 值; // 属性和方法前必须加this，代表当前对象
  this.方法 = function() {}
} // 构造函数不需要return就可以返回结果
// 使用
new 构造函数名(); // 必须用new来调用构造函数，返回的是创建的对象
```
3. new的执行过程
（1）new 构造函数可以在内存中创建一个空的对象
（2） this就会指向刚创建的空对象
（3）执行构造函数里面的代码，为空的对象添加属性和方法
（4） 返回新对象（所以构造函数里面不需要return）

### 1.3 使用for...in遍历对象属性
```js
for (var key in dateObj) {
  console.log(key); // key代表属性名
  console.log(dateObj[key]); // dateObj[key]代表属性值
}
```

## 2 内置对象
### 2.1 JS对象类型
- 自定义对象
- 内置对象
- 浏览器对象

### 2.2 查文档方法
最常用的是[MDN](https://developer.mozilla.org/zh-CN/)/W3C
- 看函数功能
- 看参数含义和类型
- 看返回值
- 通过demo进行测试

### 2.3 内置对象
#### 2.3.1 数学对象Math的常用属性和方法
- Math.PI：圆周率
- Math.max()
- Math.min()
- Math.abs()：求绝对值
- 三个取整方法：
  - Math.floor() 向下取整
  - Math.ceil() 向上取整
  - Math.round() 四舍五入
- Math.random()：取随机数

#### 2.3.2 日期对象Date的常用属性和方法
Date对象必须用构造函数来创建，即`new Date()`
参数说明：
```js
new Date(); // 不带参，返回当前系统时间
new Date(2020, 8, 2); // 数字型，8代表9月,返回的是“2020-09-02”
new Date("2020-8-2 7:4:0"); // 字符串型
```
1. 格式化日期
- dateObj.getFullYear()
- dateObj.getMonth()
- dateObj.getDate()
- dateObj.getDay(): 获取周几（周日0-周六6）
- dateObj.getHours()
- dateObj.getMinutes()
- dateObj.getSeconds()

2. 获得自1970-1-1的总毫秒数（时间戳）
```js
var date = new Date();
// 1. 通过valueOf()或getTime()
date.valueOf();
date.getTime();
// 2. 简单写法（最常用）
+new Date();
// 3. H5新增的 Date.now()
```

#### 2.3.3 数组对象Array的常用属性和方法
1. 创建数组对象的两种方法
- 利用数组字面量：`var arr = [1,2,3]`
- 利用构造函数: `var arr = new Array()`
```js
new Array(length);  // 1个参数代表元素个数
new Array(2,3); // 2个以上代表数组元素
```
2. 检测是否为数组的两种方法
- instanceof 运算符（返回bool值）： `obj instanceof Array`
- Array.isArray() 方法： `Array.isArray([1, 2, 3])`
3. 增加元素的方法
- push(args)：在数组尾部添加1个或多个元素，返回新数组的长度
- unshift(args)：在数组开头添加1个或多个元素，返回新数组的长度
4. 删除元素的方法
- pop(): 删除数组最后的元素，返回被删除的元素
- shift(): 删除数组第一个元素，返回被删除的元素
5. 数组排序方法
- reverse()：反转数组
- sort(comparefunc)：排序数组
```js
arr.sort(function (a, b) {
  return a-b; // 当返回值<0时，a会排在b的前面，升序排序
  // return b-a; // 降序排序
})
```
6. 数组索引方法
- indexOf(item, [startFrom]): 返回数组元素的第一个满足条件的索引号，没有返回-1，可选参数startFrom可指定起始位置
- lastIndexOf(): 返回数组元素最后一个满足条件的索引号，没有返回-1

【应用案例】：数组去重
核心思想：遍历旧数组，拿每一个元素去新数组中查，如果没有则加入新数组
```js
var arr = ['a', 'b', 'a', 'c'];
var newArr = []
for (var i=0; i<arr.length; i++) {
  if(newArr.indexOf(arr[i]) != -1) {
    newArr.push(arr[i]);
  }
}
```
7. 数组转字符串
- toString()方法：`arr.toString()`, 默认用','连接字符串
- join(分隔符)方法：可以指定分隔符

8. 基本包装类型
将简单数据类型包装成复杂数据类型，这样简单数据类型就有属性和方法了，JS对String,Number和Boolean都有包装
```js
var str = "blate";
console.log(str.length); // 会输出5，普通字符串本来是没有length属性的，能够输出是因为JS做了包装
// 等价于如下过程
var tmp = new String("blate"); // 生成临时变量进行包装
str = tmp; // 赋值
tmp = null; // 销毁临时变量
```

#### 2.3.4 字符串对象
1. 字符串的不可变性：对字符串的任何修改操作都会返回新的字符串，不适合做大量拼接，很消耗内存
2. 根据字符返回位置，同数组的indexOf()和lastIndexOf()方法
3. 根据位置返回字符：
  - charAt(idx)
  - charCodeAt(idx)返回索引位置字符的ASCII码，方便检测用户按键
  - str[idx] H5新增（IE8+支持）

【应用案例】找出出现次数最多的字符
核心思路：利用对象建立字符和出现次数的哈希映射，如果对象obj存在字符属性ch,obj[ch]能够通过if判断，最后遍历对象属性找到出现最多的字符
```js
var str = "blateyang";
var obj = {};
for(var i=0; i<str.length; i++){
        var ch = str.charAt(i);
        if(!obj[ch]) {
                obj[ch] = 1;
        }else{
                obj[ch]++;
        }
}

var max = 0;
var maxCh = "";
for(k in obj){
        if(obj[k] > max){
                max = obj[k];
                maxCh = k;
        }
}
console.log(maxCh + ":" + max);
```

4. 字符串的拼接和截取
- concat(str1, str2...)：拼接字符串<=>str1 + str2
- substr(startFrom, num): 截取从startFrom开始的num个字符串

5. 字符串的替换和转数组
- str.replace(srcChar, dstChar)：用dstChar替换str中的srcChar字符，只替换一次
- str.split(delimit)：将str按分隔符delimit转成数组

6. 大小写转换
- toUpperCase()：转大写
- toLowerCase()：转小写

### 3 简单数据类型和复杂数据类型
#### 3.1 数据类型内存分配
- 简单数据类型（又称值类型）：在存储时变量存的是值本身，有5种：string, number, boolean, undefined, null（空对象）
- 复杂数据类型（也称引用类型）：在存储时变量存的是地址（引用），该地址指向存放在堆中的实际复杂数据
#### 3.2 简单数据类型的参数传递
采用值传递的方式
```js
function testArgs(argu) {
    argu++;
    return argu;
}

argu = 1;
console.log(testArgs(argu)); // 2
console.log(argu); // 1
```
#### 3.3 复杂数据类型的参数传递
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
