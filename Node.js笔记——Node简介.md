## 1 进程和线程
## 1.1 进程
- 为程序运行提供必备的环境，是资源分配的最小单位
- 类似工厂中的车间

## 1.2 线程
- 线程是计算机中最小的执行单位，线程负责执行进程中的程序
- 类似工厂中的工人

通常来讲，多线程要好于单线程，执行效率更高，但也有缺点就是容易发生资源争抢。对于相对简单的任务，单线程更合适。

## 2 Node.js简介
Node.js是一个能在服务器端运行JS的开源、跨平台JS运行环境。
- 技术特点：采用Chrome V8引擎运行JS代码，使用**事件驱动（单线程）**,**非阻塞**和**异步I/O模型**等技术来提高性能。
- 核心模块：文件系统I/O、网络、数据流等
- 常用的Node框架：Express.js
- 发明人：瑞恩达尔(Ryan Dahl)


## 3 COMMONJS规范
弥补JS没有模块化标准和管理系统的缺陷

### 3.1 模块引用
使用require()方法将一个模块引入到当前运行环境
`var math = require('math')`

### 3.2 模块定义
exports对象用于导出当前模块的方法或变量，在node中一个文件就是一个模块
```javascript
function greet(name) {
  console.log("hello, " + name );
}
module.exports.greet = greet ; // 将greet作为模块的输出暴露出去
```
注意： 
模块中的代码都是被node包裹在`function(exports, require, module, __filename, __dirname)`中的，可通过`console.log(arguments.callee)`查看
- module: 代表模块本身
- exprots: 该对象用来将变量或函数暴露到外部，是module的属性， exports指向了module.exports
- require: 函数，用来引入模块
- __filename: 当前模块的完整路径
- __dirname: 当前模块的父目录

模块调用原理：
require(模块路径)会让Node根据路径加载对应模块对象，而事先被export的变量或函数便可以通过模块对象获取到
### 3.3 模块标识
就是模块的名字，即传递给requires()方法的参数，通常是js文件的文件名，需符合驼峰命名法（可以包含./或../等相对路径）

模块分类：
  - 核心模块：由Node引擎提供的模块，如fs
  - 文件模块：用户自己创建的模块，标识为文件的绝对或相对路径
### 3.4包package
包规范将相关模块组合到一起，用来形成一组完整的工具，由包结构和包描述文件组成。
- 包结构
```
-package.json 描述文件
-bin  可执行二进制文件
-lib  js代码 
-doc  文档
-test 单元测试
```
- 包描述文件 
用于表达非代码相关的信息

### 3.5 NPM包管理器
是COMMONJS规范的实践，帮助Node完成第三方模块的发布、安装和依赖等。
常用npm命令：
- npm -v,查看版本
- npm,帮助说明
- npm install 包名,在当前目录安装包
- npm install 包名 -g,全局模式安装包
- npm install 包名 --registry=地址,从镜像源安装
- npm install 文件路径,从本地安装
- npm remove 包名,删除包


## 4 Buffer(缓冲区)
类似java的字节数组byte[]，可以通过Buffer来创建内存中的空间

### 4.1 Buffer的操作
- 保存字符串 
```
let str = "hello";
let buf = Buffer.from(str, "utf-8");
```
- 创建指定大小的Buffer对象
`let buf = Buffer.alloc(1024*8) // 8kb`

### 4.2 Buffer的转换
- 字符串转Buffer `Buffer.from(str, [encoding])`
- Buffer转字符串 `buf.toString([encoding], [start], [end])`

### 4.3 写入操作
- 向缓冲区写入字符串 `buf.write(string [,offset, length, encoding])`

### 4.4 读取操作
- 读取并转成字符串 `buf.toString([encoding, start, end])`
- 读取指定位置 `buf[index]`

### 4.6 其它操作
- 复制缓冲区 `buf.copy(target [,targetStart, SrcStart, SrcEnd])`  

### 4.7 Node.js在VSCode中的运行和调试
#### 4.7.1 命令行运行
在VSCode终端cd到要执行的文件下，用node运行js文件
```
PS F:\Guangjun\web-projects\JavaScript-exercises\Node练习> node test.js
Debugger listening on ws://127.0.0.1:63513/f4cedf8c-105c-4a9b-a909-49a0aacd3fe1
For help, see: https://nodejs.org/en/docs/inspector
Debugger attached.
这是异步写入
Waiting for the debugger to disconnect...
```
#### 4.7.2 调试控制台调试
    1. 在代码中打好断点
    2. 点击VSCode右侧的"运行"按钮
    3. 选择Node.js执行环境
    4. 在调试控制台查看输出结果
## 5 fs文件系统
服务器的主要工作就是将本地的文件发送给远程的客户端，因此文件系统是很重要的，Node通过fs模块和文件系统交互
使用fs模块需要先加载`const fs = requires("fs");`

### 5.1 同步和异步调用
fs模块中的文件操作都有**同步**和**异步**两种形式可供调用
- 同步形式会阻塞程序的执行直到文件操作结束
- 异步形式不会阻塞程序的执行，文件操作结束后会通过回调函数将结果返回

作为服务器为了同时服务多个用户，一般必须采用异步执行的方式，只有在服务器启动和停止时才可以用同步方式。
### 5.2 开关文件
- 打开文件
  - fs.openSync(path, flags[, mode]); // flags:打开文件要做的操作类型，e.g. r,w,a; mode:设置文件的操作权限，一般不传; 会返回文件描述符fd
  - fs.open(path, flags[,mode], callback); //  异步调用不会返回文件描述符fd, 通过回调函数的参数arguments获得返回结果，第一个参数是错误对象err, 第二个参数是文件描述符fd
- 关闭文件
  - fs.closeSync(fd)
  - fs.close(fd, callback)
  
以上函数和同步文件写入（fs.writeSync)、异步文件写入(fs.write)、同步文件读取(fs.readSync)、异步文件读取(fs.read)结合使用
### 5.3 简单文件写入
- fs.writeFileSync(file, data[,options]]); // file:文件路径，data: 要写入的数据（String或Buffer); options:json对象，包含属性（encoding, flag, mode)
- fs.writeFile(file, data[,options], callback); 


### 5.4 流式文件写入
适用于写入大量数据到文件
1. 先创建Writable对象： ws = fs.createWriteStream(path[, options]) 
2. 用write方法写入： ws.write(data[,position[,encoding]]); // position:写入的起始位置
3. 用end方法关闭: ws.end();

### 5.5 简单文件读取
- fs.readFileSync(file, [,options]]); // options:json对象，包含属性（encoding, flag, mode)
- fs.readFile(file, [,options], callback); // callback: 回调函数，2个参数：err和data

### 5.6 流式文件读取
适用于读入大量数据
1. 先创建Readable对象： rs = fs.createReadStream(path[, options]) 
2. 绑定'data'读取： rs.on('data', callback(bytesRead)); // bytesRead已读取的字节数
3. 绑定'end'读完关闭: rs.on('end', callback);
4. 绑定'error'错误处理： rs.on('error', callback);

### 5.7 其它操作
- 验证路径是否存在
  - fs.existSync(path)
- 获取文件信息
  - fs.stat(path, callback)
  - fs.statSync(path)
- 删除文件
  - fs.unlink(path, callback)
  - fs.unlinkSync(path)
- 列出文件  
  - fs.readdir(path[,option], callback)
  - fs.readdirSync(path[,option])
- 拷贝文件
```javascript
// 利用pipe将输入流和输出流串接起来
let rs = fs.createReadStream("sample.txt");
let ws = fs.createWriteStream("copy.txt");
rs.pipe(ws);
// 默认rs流读完后会自动关闭ws流，如果不希望关闭，可采用如下写法
rs.pipe(ws, {end: false});
```
- 建立目录
  - fs.mkdir(path[,mode], callback)
  - fs.mkdirSync(path[,mode])
- 删除目录
  - fs.rmdir(path, callback)
  - fs.rmdirSync(path)
- 重命名文件和目录
  - fs.rename(oldPath, newPath, callback)
  - fs.renameSync(oldPath, newPath)
