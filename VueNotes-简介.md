# 1 Vue核心
## 1.1 Vue的基本认识
### 1.1.1 Vue是什么？
Vue(读音/Vju:/)是用于动态构建用户界面的**渐进式**前端框架，开发者是尤雨溪。渐进式的含义是
>[Vue可以自底向上被逐层应用（增量开发），不像其它前端框架（Angular,React)有较强的主张（用Angular必须使用它的模块机制和依赖注入，用React需要接受它的函数式编程理念），Vue主张最少，没有多做职责之外的事(Vue2.0 中，“渐进式框架”和“自底向上增量开发的设计”这两个概念是什么？ - 徐飞的回答 - 知乎)](https://www.zhihu.com/question/51907207/answer/136559185)

### 1.1.2 Vue的特点
- 遵循MVVM模式（Model-View-ViewModel)
- 编码简洁，易于上手， 体积小，运行效率高，适合移动/PC端开发
- 核心库只关注视图层，便于和第三方库或既有项目集成

### 1.1.3 与其它前端JS框架的关系
- 借鉴Angular的模板和数据绑定技术
- 借鉴React的组件化和虚拟DOM技术
注：虚拟DOM是指利用js在内存中创建一个虚拟的DOM对象，具有和实际DOM一样的树形目录结构，但不会进行排版与重绘操作，修改的效率更高
### 1.1.4 Vue扩展插件
- vue-cli:vue脚手架
- vue-resource/axios: ajax请求
- vue-router: vue路由
- vuex: vue状态管理
- mint-ui: 基于vue的UI组件库（移动端）
- element-ui: 基于vue的UI组件库（PC端）

## 1.2 Vue的基本使用
### 1.2.1 像引入js库一样引入Vue.js
- 方式1（开发环境版，包含了命令行警告，便于调试）：`<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>`
- 方式2（生产环境版，优化了尺寸和速度）：`<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

### 1.2.2 helloworld示例
```html
<div id="app">
  <input type="text" v-model="username">
  <p>hello, {{username}}</p>
</div>

<script type="text/javascript" src="../js/vue.js"></script>
<script>
 new Vue({
   el: "#app",
   data: {
     username: "ygj"
   })
 }
</script>
```

### 1.2.3 理解MVVM模式
- 通过DOM Listeners将视图绑定到模型：
`View(DOM)--->ViewModel(DOM Listeners)--->Model(JS对象)`
- 通过数据绑定将模型绑定到视图：`View(DOM)<---ViewModel(Data Bindings)<---Model(JS对象)`


## 1.3 模板语法
### 1.3.1 模板的理解
1. 动态的html页面
2. 包含了一些js语法代码：a. 双大括号表达式 b. 指令（以v-开头的自定义标签属性）

### 1.3.2 双大括号表达式
- 语法：{{exp}}
- 作用：向页面输出数据
- 可以调用对象的方法

### 1.3.3 指令1：v-bind 绑定变化的属性
- 语法：`v-bind:xxx = 'yyy'` // 简写: `:xxx='yyy'`
- 作用：指定变化的属性值

### 1.3.4 指令2：v-on 绑定事件监听
- 语法：
```
v-on:keyup='fun' // 简写：@keyup='fun'
v-on:keyup='fun(参数)'
v-on:keyup.enter='fun' // 简写：@keyup.enter='fun'
```
- 功能：绑定指定事件名的回调函数

### 1.3.5 指令3：v-model 在表单控件或者组件上创建双向绑定
- 语法：
```html
<div id="app">
  <input type="text" v-model="username">
  <p>hello, {{username}}</p>
</div>
```
表单值会随着用户输入而改变，同时页面显示也会随着表单值而改变
## 1.4 计算属性和监视
```html
<div id="demo">
  姓:<input type="text" placeholder="FirstName" v-model="firstName"><br>
  名:<input type="text" placeholder="LastName" v-model="lastName"><br>
  姓名1(单向):<input type="text" placeholder="FullName" v-model="fullName1"><br>
  姓名2(单向):<input type="text" placeholder="FullName" v-model="fullName2"><br>
  姓名3(双向):<input type="text" placeholder="FullName2" v-model="fullName3"><br>
</div>
  <script type="text/javascript" src="../js/vue.js"></script>

```
### 1.4.1 计算属性
  - 在computed属性对象中定义计算属性的方法
  - 在页面中使用`v-model=方法名`或`{{方法名}}`显示计算结果

```javascript
 var vm=new Vue({
    el:'#demo',
    data:{
    firstName:'Kobe',
    lastName:'bryant',
    fullName2:'Kobebryant'
    },

    computed:{
      fullName1: function() {
        return this.firstName + " " + this.lastName;
      }
    }
    })
```
### 1.4.2 监视属性
  - 通过watch配置或Vue对象的$.watch()来监视属性
  - 当属性变化，回调函数自动调用

```javascript
 var vm=new Vue({
    el:'#demo',
    data:{
    firstName:'Kobe',
    lastName:'bryant',
    fullName2:'Kobebryant'
    },
    watch: {
      lastName: function(newVal) {
        this.fullName2 = this.firstName + ' ' + newVal;
      }
    }
 })
 /*
 vm.$watch('lastName', function(val) {
   this.fullName2 = this.firstName + ' ' + val;
 })
 */
```
### 1.4.3 计算属性高级
- 通过getter/setter对属性数据进行计算和监视
- 计算属性存在缓存，多次读取只执行一次getter计算
```javascript
 var vm=new Vue({
    el:'#demo',
    data:{
    firstName:'Kobe',
    lastName:'bryant',
    fullName2:'Kobebryant'
    },
    computed:{
      fullName1: function() {
        return this.firstName + " " + this.lastName;
       }, //改动fullName1中表单的值,firstName和lastName不会变

       fullName3: {
         // 类似computed
         get: function() {
          return this.firstName + " " + this.lastName;
         },
         // 类似watch
         set: function(value) {
          var names = value.split(' ');
          this.firstName = names[0];
          this.lastName = names[1];
         } // 改动fullName3中表单的值,firstName和lastName会跟着变
       },
    }
 })
```

## 1.5 class和style绑定
- 应用场景：网页中的样式有时候需要动态变化
- class/style绑定是专门用来实现动态样式的技术
1. class绑定
`:class='xxx'`
  - 表达式xxx是字符串：`classA`
  - 表达式xxx是js对象：`{classA: isA, classB: isB}`
  - 表达式xxx是js数组：`['classA', 'classB']`
2. style绑定
  - `:style="{color: activeColor, fontSize: fontSize+'px'}`
  - activeColor,fontSize是data的属性

## 1.6 条件渲染
1. v-if和v-else

2. v-show
v-show为false表示不显示，但还是在内存里面
```html
  <div id="demo">
    <h2 v-if="ok">表白成功</h2>
    <h2 v-else>表白失败</h2>
    <h2 v-show="ok">求婚成功</h2>
    <h2 v-show="!ok">求婚失败</h2>
    <button @click="ok=!ok">切换</button>
    <!--或者下面，注意button一定要放在div#demo里面，否则事件不起作用-->
    <button @click="fun">切换</button>
  </div>

  <script type="text/javascript" src="../js/vue.js"></script>
  <script>
   var vm=new Vue({
    el:'#demo',
    data:{
      ok: false
    },
    methods: {
      fun: function() {
        this.ok = !this.ok;
      }
    }
  })
  </script>
  ```

3. 二者比较
  - 如需频繁切换，使用v-show较好
  - v-if如果不成立，其所有的子节点都不会解析


## 1.7 列表渲染
1. 列表显示指令
  - 数组：v-for/index
  - 对象： v-for/key
2. 列表删除和替换：常利用splice(idx, deleteNum, arr)
3. 列表的高级处理
  - 列表过滤
  - 列表排序
常利用filter和=>箭头函数

注意点：
- vue本身只是监视了列表对象的改变，并没有监视对象内部数据的改变
- vue重写了数组中一系列改变内部数据的方法（称为变异方法，分2步：1.实现方法的原生功能 2.更新界面）
因此，更新列表对象中的元素，使用`this.objList[idx] = newVal`无效，需要使用`this.objList.splice(idx, 1, newVal)`才有效

## 1.8 事件处理
### 1.8.1 绑定监听
- v-on:xxx='fun'
- @xxx='fun'
- @xxx='fun(参数)'
- 默认形参event。event.target指向事件发生的元素对象，当要传递包含event在内的多个参数时，用$event作为实参传递给event
### 1.8.2 事件修饰符
1. .prevent: 阻止事件的默认行为 event.preventDefault()
2. .stop: 停止事件冒泡 event.stopPropagation()
注: 事件冒泡是指在DOM的嵌套结构中，事件传播的顺序，有2种：冒泡和捕获。在IE中，事件发生遵循由具体（最内）->抽象（最外），就像冒泡一样；在firefox中，事件发生则按照捕获的顺序，即由抽象（最外）->具体（最内）的顺序执行。
### 1.8.3 按键修饰符
1. .keyCode: 操作的是某个keyCode值的按键
2. .keyName: 操作的是某个按键名的按键（如.enter代表enter键)

## 1.9 使用v-model对表单数据自动收集
```html
<input type="text" v-model="user.username">
<textarea v-model="user.desc" rows="10"></textarea>
<input type="radio" value="male" v-model="user.sex">
<input type="checkbox" value="football" v-model="user.likes">
<input type="checkbox" value="basketball" v-model="user.likes">
<select v-model="user.cityId">
  <option v-for="city in allCitys" :value="city.id">
  {{city.name}}
  </option>
</select>
```

## 1.10 Vue对象生命周期
### 1.10.1 生命周期的三个阶段
1. 创建(初始化显示)
  - beforeCreate()
  - created(): 创建完响应式实例
  - beforeMount()：在内存中编译模板
  - mounted():（将模板）挂载（初始化）到DOM树上后立即显示（1次）
2. 工作（更新显示）：this.xxx = value
  - beforeUpdate()
  - updated()
3. 销毁vue实例：this.$destroy()
  - beforeDestroy(): 死亡之前调用（1次）
  - destroy()
![Vue生命周期](https://cn.vuejs.org/images/lifecycle.png)
图片来自Vue中文文档，图中Compile el's outerHTML应改为Compile el's innerHTML
### 1.10.2 常用的生命周期方法
- mounted():发送ajax请求，启动定时器等异步任务
- beforeDestroy():做收尾工作，如清除定时器
```javascript
new Vue() {
  el: 'div',
  data: {
    msg: 'xxx',
    isShowing: true
  },
  created() {
    this.intervalId = setInterval(() => {this.isShowing=!this.isShowing;}, 1000); 
    // 直接在匿名函数中写this.isShowing=!this.isShowing识别不到正确的this,匿名函数中的this会被视作window，而使用箭头函数会沿着this的作用域链往外找（与this.intervalId指向同一个对象，即调用created的vue实例），但对created不要使用箭头函数（其中的this指向的是window)
  }，
  beforeDestroy() {
    clearInterval(this.intervalId);
  }
}
```
## 1.11 过渡和动画
### 1.11.1 理解
1. 本质是操作css的transition或animation
2. vue会给目标元素添加或移除特定的样式class
### 1.11.2 过渡的相关类名
  - xxx-enter/xxx-leave-to:指定隐藏时的样式
  - xxx-enter-active: 指定显示时的transition
  - xxx-leave-active: 指定隐藏时的transition
![过渡图示](https://cn.vuejs.org/images/transition.png)
图片来自Vue中文文档
### 1.11.3 编码指导
1. 在目标元素外包裹`<transition name="xxx">`
2. 定义class样式:
  - 指定过渡样式 transition
  - 指定隐藏时的样式：opacity或其他
```css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}

.fade-enter, .fade-leave-to {
  opacity: 0
}
```
## 1.12 过滤器
### 1.12.1 理解过滤器
- 功能：对要显示的数据进行特定格式化处理后再显示
- 注意：并没有改变原数据，而是产生了新的对应数据
### 1.12.2 定义和使用过滤器
1. 定义过滤器
```javascript
Vue.filter(filterName, function(value[,arg1, arg2, ...]) {
  // 进行一定的数据处理
  return newValue;
})
```
2. 使用过滤器
过滤器可以用在两个地方：双花括号插值和v-bind表达式
```html
<div>{{myData | filterName}}</div>
<div>{{myData | filterName(args)}}</div>
```

## 1.13 内置指令和自定义指令
### 1.13.1 内置指令
- v-text: 更新html元素的textContent属性
- v-html: 更新html元素的innerHTML属性
- v-if/v-else: 条件显示
- v-show: 通过控制元素的display属性显示/隐藏
- v-for: 用于数组、对象属性遍历
- v-on: 绑定事件
- v-bind: 绑定解析表达式
- v-model：双向数据绑定
- v-cloak: 防止闪现表达式(利用vue指令属性在解析之前存在，但在解析之后便会被去掉的原理实现)
- v-pre: 原样输出
- ref: 为某个元素注册一个唯一标识，vue对象通过`$refs.标识名`访问这个元素对象
### 1.13.2 内置指令用法举例
- v-cloak
```html
<head>
  <style>
    [v-cloak] { /* 属性选择器 */
      display: none
    }
  </style>
</head>
<body>
  <div id="test">
    <p :value="msg" v-cloak>{{msg}}</p>
  </div>
  <script>
    alert("wait");
    new Vue({
      el: "#test",
      data: {
        msg: "hello"
      }
    })
  </script>
</body>
```
### 1.13.2 自定义指令
- 应用场景：内置指令无法满足需求而又需要频繁使用的功能组件
- 注册全局指令（所有vue对象的el都可以使用）
`Vue.directive('upper-text', function(el, binding) {el.innerHTML = binding.value.toUpperCase()})`
- 注册局部指令（只有当前对象的el才可以使用）
```javascript
  directives: {
    'upper-text': {
      bind(el, binding) {
        el.innerHTML = binding.value.toUpperCase();
      }
    }
  }
```
- 使用指令：`v-upper-text='xxx'`

## 1.14 自定义插件
- Vue插件是一个包含install方法的对象，通过install方法给Vue或Vue实例添加全局方法、实例方法（添加到Vue.prototype上）和自定义全局指令
- 自定义插件的情况不多，主要是会使用插件，在引入插件js文件后，要通过`Vue.use(插件名)`来使用插件