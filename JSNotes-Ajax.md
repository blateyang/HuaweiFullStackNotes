## 1 初识AJAX 
AJAX是Asynchronus JavaScript And XML的简称（标准发音为A jax，国内常读作阿贾克斯)，它可以在不刷新整个网页的情况下异步地和服务器进行数据通信来进行局部更新
### 1.1 AJAX技术的优势
- 大幅提升用户体验（表单异步验证的例子）
- 能够减轻服务器端的压力（异步分部分加载服务器数据）

- 异步加载（异步与服务器通信）
- 局部更新（无刷新更新数据）
- 前端和后端负载均衡


### 1.2 网络通信
#### 1. OSI七层模型（自顶向下）
- 应用层：为应用程序提供服务
- 表示层：进行编码转换、数据加密等
- 会话层：建立、管理和维护会话
- 传输层：建立、管理和维护端到端的连接（相当于寄快递时填写寄件人和收件人的手机号）>TCP&UDP
- 网络层：IP选址和路由选择（相当于寄快递时填写收发地址）>路由器
- 数据链路层：提供介质访问和链路管理，对二进制比特流进行分帧（类似汉字的断句）>交换机
- 物理层：传输二进制信号

#### 2. TCP/IP协议
传输控制协议/网络协议

#### 3. HTTP协议
  特点：
- 无状态协议
- 不会建立持久连接

  HTTP通信机制：基于请求-响应模式


## 2 Node.js服务
Node.js是基于Chrome V8引擎的javascript运行时，类似java中包含jvm的jre，简单来讲，Node.js就是运行在服务端的js。

### 2.1 安装和验证
官网下载安装包直接安装，可通过node -v和npm -v验证是否安装成功
npm是node的包管理器，用npm安装的依赖包通常放在国外的网站，可利用淘宝的cnpm代替npm进行安装
安装cnpm的命令为 `npm install -g cnpm --registry=https://registry.npm.taobao.org`
### 2.2 koa2框架
koa是Node.js的下一代Web框架（类似java中的tomcat)，借助大量中间件（类似插件），koa代码量非常少，分2个版本koa v1和koa v2

### 2.3 搭建web服务器
- 使用`cnpm install koa-generator -g`可以安装koa-generator脚手架，方便生成项目
- 使用koa2命令创建项目 `koa2 项目名`
- 使用cnpm install安装依赖包
- 使用npm start运行项目

## 3 揭秘AJAX
### 3.1 XHR(XMLHttpReaquest)
方法：
- open() 初始化
- send() 发送
- setRequestHeader Content-type
属性：
- onreadystatechange
- readyState
- status
- responseText
### 3.2 XHR四步走
1. 创建XHR 
2. 使用open方法，初始化请求参数 
```js
open(method, url, async);
setRequestHeader("Content-type", "application/x-www-form-urlencoded");
```
3. 使用onreadystatechange属性，设置处理返回数据的回调
4. 使用send方法，发送请求 `send(param)`
- readyState: 4(代表XHR对象已处于数据已返回的就绪状态)
- status: 200（代表请求成功）
```js
var xhr = null;
// 1. 创建xhr对象
if(window.XMLHttpRequest) {
  xhr = new XMLHttpRequest();
}else if(window.ActiveXObject) {
  xhr = new ActiveXObject(); // 兼容IE8以下的低版本浏览器
}

// 2. 初始化xhr
xhr.open("get", "/ajax", true);
xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

// 3. 设置响应回调
xhr.onreadystatechange = function() {
  if(xhr.readyState == 4) {
    if(xhr.status == 200) {
      console.log(xhr.responseText);
    }
  }
}
// 4. 发送请求
xhr.send();

```

### 3.3 跨域请求
1. 同源策略
- 浏览器的安全策略
- 规则：依据**协议名+主机名/IP+端口号**判断是否同源
2. 跨域请求
- 跨域的方法：JSONP, Proxy, [iframe（要结合window.name)](https://www.cnblogs.com/happy-8090/p/11570998.html), CORS
- 天然可跨域的标签：script(image,link)

常用的是通过JSONP跨域，其原理是利用天然可跨域的script的src属性拿到不同源的数据
假设网站1针对/ajax请求会返回`f("来自站点1")`，
如果在网站2直接用XHR技术访问网站1的url，会报跨域错误，而在网站2的页面有如下一段
```html
<script>
  function f(str) {
    console.log(str);
  }
</script>
<script src="站点1url/ajax"></script>
```
就可以在网站2的控制台打印出“来自站点1”