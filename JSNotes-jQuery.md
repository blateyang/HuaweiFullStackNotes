## 1 jQuery简介
### 1.1 什么是jQuery？
js的函数库，宗旨：write less, do more，用'$'标识
### 1.2 get和post方法
1. get方法用于获取资源
```javascript
$.get(url, data, callback, type)
```
- url: 代表请求的资源
- data: 代表请求的参数
- callback: 是请求成功时的回调函数
- type: 请求返回数据的格式
- 除了url，其它3个参数没有时可以省略

**注意**：
- 请求的参数可以是json也可以是字符串
- 返回的数据格式可以是：json,html,xml,text,script等

2. post方法用于增删改资源
```javascript
$.post(url, data, callback, type)
```
参数含义同上
**注意**：
- post的请求参数是放在请求体里面的

### 1.3 ajax方法
1. 作用：是get和post方法的基础，较为灵活，可以自定义请求
1. 语法格式`ajax(option)`, option是json格式的配置参数，用于设置ajax请求
2. option配置参数
常用配置
- url: 代表请求的资源
- type: 请求方式（get或post等)
- data: 代表请求的参数
- dataType: 请求返回数据的格式
- succuss: 请求成功后的回调函数
- error: 请求失败后的回调函数
- complete: 请求完成的回调函数（不论成功或失败）

bool类型的配置
- async: 是否为异步请求
- cache: 是否进行缓存（主要针对get请求）

容易被忽视的配置项
- timeout: 请求超时时间(ms)