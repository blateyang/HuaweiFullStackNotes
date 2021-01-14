# 1 创建、编写和打包项目
## 1.1 使用vue-cli创建项目模板
### 1.1.1 说明
vue-cli是官方提供的脚手架工具，用来从github.com/vuejs-templates下载模板
## 1.1.2 创建项目 
```
npm install -g vue-cli 
vue init webpack vue_demo 
cd vue_demo 
npm install
npm run dev
```

## 1.2 项目的打包与发布
### 1.2.1 打包
`npm run build`
### 1.2.2 发布
1. 使用静态服务器工具包
```
npm install -g serve
serve dist
//访问localhost:5000
```
2. 使用动态web服务器（tomcat)
- 修改配置文件：webpack.prod.conf.js
`output: {publicPath: '/xxx/' //打包文件夹的名称}`
- 重新打包
- 修改dist文件夹为项目名称:xxx
- 将xxx拷贝到tomcat的webapps下（相当于一个静态的web应用）
- 启动tomcat，访问localhost:8080/xxx

## 1.3 eslint
eslint是代码规范检查工具，可以检查ES，JSX, css等是否合规及语法错误，也支持自定义错误
和提示
### 1.3.1 规则错误的三种等级
- off/0：关闭规则
- warn/1：打开规则，并作为警告
- error/2：打开规则，并作为错误
### 1.3.2 相关配置文件
- .eslintrc.js:全局规则配置文件
```
'rule': {
  no-new(规则名): 1
}
```
### 1.3.3 禁用规则
/* eslint-disable */

# 2 组件化编程
## 2.1 组件定义与使用
### 2.1.1 什么是Vue组件？
.vue后缀的文件，由html(template),css和js三部分组成，是可复用的界面功能模块
1. 模板页面
```html
<template>
页面模板
</template>
```
2. JS模块对象
```html
<script>
  export default {
    methods: {},
    computed: {},
    components: {}
  }
</script>
```
3. 样式
```html
<style>
  样式定义
</style>
```
### 2.1.2 基本使用
1. 引入标签
2. 映射成标签组件
3. 使用组件标签
```html
<template>
  <HelloWorld/> 
</template>

<script>
  import HelloWorld from "./components/HelloWorld"; // 引入标签
  export default {
    components: {
      HelloWorld // 映射成标签组件
    },
    template: '<HelloWorld/>'
  }
</script>
```

## 2.2 组件间通信
### 2.2.1 组件间通信基本原则
1. 不要在子组件中直接修改父组件的状态数据 
2. 数据在哪，更新数据的行为（函数）就应该定义在哪

### 2.2.2 vue组件间通信方式
1. props: 通过组件标签属性通信，用于父组件向子组件传递数据，向非子后代传递数据需要多层逐层传递，且兄弟组件间不能直接props通信
2. vue的自定义事件：用于子组件向父组件传递消息（数据），隔代组件和兄弟组件间不适用
3. 消息订阅与发布（如：pubsub-js库）：可实现任意关系组件间通信
4. slot: 用于父组件向子组件传递整个'标签'
5. vuex：vue状态管理工具,用于多个组件在操作共享数据时的数据管理（对vue应用中多个组件的共享状态进行集中式的读写管理）
### 2.2.3 消息订阅与发布（pubsub库）
1. 订阅消息(绑定事件监听)
`PubSub.subscribe('消息名', function(msg, data) {}) `
注意：// msg接收的是消息名，function是被PubSub调用的回调函数，为了能拿到所在组件的this指针，需要用箭头函数
2. 发布消息（触发事件）
`PubSub.publish('消息名', data)`

### 2.2.4 slot传递标签
子组件child.vue
```html
<template>
  <div>
    <slot name='xxx'>不确定的标签结构（类似占位符）</slot>
    <div>组件确定的标签结构</div>
    <slot name='yyy'>不确定的标签结构（类似占位符）</slot>
  </div>
</template>
```
父组件标签parent.vue
```html
<child>
  <div slot='xxx'>xxx对应的标签结构</div>
  <div slot='yyy'>yyy对应的标签结构</div>
</child>
```

## 2.3 组件化编程思想
1. 拆分页面为逻辑块(自顶向下拆分)
2. 确定静态组件
3. 确定动态组件（动态初始化数据，动态更新）
注意：
关于数据在何处定义的问题，如果数据只被一个组件使用，就定义在它里面，如果被多个组件使用，就定义在它们的父组件中
### 2.3.1 例子1：初始化显示（静态部分）
### 2.3.2 例子2: 交互添加
0. 绑定事件
1. 检查输入合法性
2. 根据输入生成一个对象
3. 添加到对象数组中
4. 清除输入

小知识点：onmouseenter,onmouseleave,onmouseover,onmouseout区别
onmouseenter/onmouseleave对应的是过程，onmouseover/onmouseout对应的是状态

### 2.3.3 例子3：存储数据
- 需要用到的浏览器API——window.lcoalStorage
- 需要用到的技术——深度监视（深度：不仅要监视对象表面，还要监视内部）
1. 存：页面有改变就需要将状态保存到本地文件
```javascript
watch: {// 监视
  data: {
    deep: true, // 深度监视
    handle: function(newVal, oldVal) {
      window.localStorage.setItem("data_key", JSON.stringify(data)) // 将data_key的最新值保存到localStorage中
    }
  }
}
```
2. 读：打开网页时从本地文件读取数据
`JSON.parse(window.localStorage.getItem("data_key") || '[]')`

### 2.3.4 例子4：自定义事件（用于子组件向父组件传递数据）
1. 先在父组件的标签上利用@符号绑定事件
'<TodoHeader @addTodo="addTodo">`
2. 再在子组件中触发事件并传递数据
`this.$emit("addTodo", data)`

### 2.3.5 例子5：消息订阅和发布
利用pubsub-js库的PubSub对象
1. 订阅消息对应绑定事件监听
```
PubSub.subscribe("deleteTodo", (msg, index) => { // 不能使用function(msg, index)形式，因为下面的this就会变成指向PubSub对象了
  this.deleteTodo(index)
})
```
2. 发布消息对应触发事件
```
PubSub.publish("deleteTodo", data)
```

# 3 vue-ajax
## 3.1 vue项目中两个常用的ajax库
1. vue-resource: vue插件，vue1.x使用广泛
2. axios: 通用ajax请求库，vue2.x使用广泛

## 3.2 下载
`npm install axios --save // save表示将下载的库添加到package.json的dependencies（运行时依赖）中`
## 3.3 axios使用方法
```javascript
// 引入模块
import axios from "axios"
 
// 发送ajax请求
axios.get(url)
  .then(response => {
    console.log(response.data)
  })
  .catch(error => {                                                                                             
    console.log(error.message)
  })
```

# 4 ui组件库
## 4.1 常用ui组件库
1. Mint UI: 饿了么开源的移动端UI组件库
2. Element UI: 饿了么开源的PC端UI组件库

## 4.2 使用babel可以实现按需打包
需要下载babel-plugin-component，然后修改babel配置

# 5 vue-router
## 5.1 理解
web中路由是什么？

路由是一种键值映射关系，分为前台路由和后台路由。路由的key是path(访问的url路径)，后台路由的value是处理请求的回调函数，前台路由的value是组件

## 5.2 编写使用路由的三步
1. 定义路由组件
```javascript
// 通常定义在/src/router/index.js中
export default new VueRouter({
  routes: [
    {
      path: "/home",
      component: Home,
      children:[ // 嵌套路由
        {
          path: "news",
          component: News
        },
        {
          path: "message",
          component: Message
          children:[
            { // 路由路径携带参数(param/query)
              path: "mdetail/:id", //路由组件中读取请求的参数 this.$route.params.id
              component: MessageDetail
            }
           ]       
        },
      ]
    },
    {
      path: "/about",
      component: About
    },
    {// 默认路由
      path: "/",
      redirect: "/about"
    }
  ]
})
```
2. 注册路由器
```javascript
// 写在main.js
import Vue from "vue"
import router from "./router"
// 创建vue配置路由器
new Vue({
  el: "#app",
  router,
})
```
3. 使用路由
```html
<div>
  <!--生成路由链接-->
  <router-link to="/about">About</router-link>
  <router-link to="/home">Home</router-link>
  <router-link :to="'/home/message/mdetail/'+m.id">{{m.title}}</router-link>
  <!--也可通过<route-view :msgId="m.id"></route-view>在路由中携带参数-->
  <!--用于渲染当前路由组件-->
  <keep-alive> <!--缓存路由-->
   <router-view></router-view>
  </keep-alive>
</div>
```
## 5.3 编程式路由导航
- this.$router.push(path): 相当于点击路由链接
- this.$router.replace(path): 用新路由替换当前路由（不能返回到当前路由界面）
- this.$router.back()/go(-1): 退回到上一个记录路由
- this.$router.go(1)：请求下一个记录路由

# 6 vue源码分析
## 6.1 准备知识
1. 将伪数组转换成新的真数组

`Array.prototype.slice.call(pseudoArr) // 伪数组不是数组,只是具有数组的一些特性，如length属性，可通过下标访问，但不具有数组方法`

说明：FuncName.call(obj)方法能够在某个对象上调用某方法，类似java反射的方法调用，另一种通过数组对象实例调用的写法是`[].slice.call()`

2. node.nodeType得到节点类型（数字）
几种常用的节点类型（可参考Node的属性）：
- Document：最大的节点，代表整个html文件
- Element：元素节点，nodeType为1
- Attr：属性节点
- Text：文本节点
3. `Object.defineProperty(obj, propertyName, {})`：给对象添加属性（指定描述符）
{}为配置对象，可指定以下属性描述符：

配置描述符
- configurable: 是否可重新定义
- enumerable: 是否可枚举（常用于`for in`语法中）
- value: 初始值
- writable: 是否可修改属性

访问描述符（双向绑定的原理）
- get：回调函数，根据其他相关属性动态计算得到当前属性（类似计算属性，Model->View的绑定）
- set: 回调函数，监视当前属性值的变化，更新其他相关的属性值（类似watch监视属性，View->Model的绑定）
e.g.
```javascript
const obj = {
  "firstName": 'A',
  "lastName": 'B'
}
Object.defineProperty(obj, 'fullName', {
  get() {
    return this.firstName + '-' + this.lastName
  },
  set(value) {
    const names = value.split('-')
    this.firstName = names[0]
    this.lastName = names[1]
  }
})
```
4. Object.keys(obj): 得到对象自身可枚举属性组成的数组

5. obj.hasOwnProperty(prop): 判断prop是否为对象自身的属性（区别于从原型链继承的属性）

6. DocumentFragment： 文档碎片（用于高效批量的更新多个节点）
- Document：对应显示的页面，包含多个element，一旦更新页面内的某个元素界面会立刻更新
- DocumentFragment：是内存中保存页面elements的一个容器对象（不与界面关联），如果更新其中的某个element，页面不更新

- 使用DocumentFragment的四步：

  (1) 创建DocumentFragment对象

  (2) 用appendChild()取出DOM元素中所有子节点保存到DocumentFragment对象中

  (3) 对取出的所有节点进行批量修改（可先转成数组再借用forEach等数组方法）

  (4) 将修改后的DocumentFragment插入到原元素中

## 6.2 数据代理
主要思想：通过一个对象代理另一个对象（通常在前面对象的内部，和事件代理有些类似）中属性的操作（读/写）
vue中的数据代理实现：vm对象代理内部data对象中属性的操作
示例代码：
```javascript
const vm = new Vue({
  el: "#test",
  data: {
    name: "testName1"
  }
})
console.log(vm.name) // 实际是通过Object.defineProperty为name属性设置了get属性标识符，将vm._data.name赋值给vm.name，vm._data是配置对象中data的引用
vm.name= "testName2" // 实际是通过Object.defineProperty为name属性设置了set属性标识符，将vm.name赋值给了vm._data.name
```

## 6.3 模板解析&数据绑定
@import "Vue技术原理.PNG"
Vue技术原理

双向数据绑定的实现原理（基于上图）：
- 建立在单向（model->view)数据绑定的基础上
- 在解析v-model指令时，给当前元素绑定input监听
- 当input的value变化时，将最新的值赋值给当前表达式所对应的data属性

# 7 vuex状态管理                                         

@import "vuex结构图.PNG"