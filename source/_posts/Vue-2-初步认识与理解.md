---
title: Vue-2-初步认识与理解
date: 2023-05-31 22:53:26
tags:
    - vue
    - JavaScript
categories: 
    - vue
cover: /images/vue_cover.png
---
# 简介
## 使用vue构建用户界面
- 构建用户界面
  - 编写结构
  基于 vue 中提供的**指令**，可以方便快捷的渲染页面的结构（乐不思蜀）。**数据驱动视图**（只要页面依赖的数据源变化，则页面自动重新渲染）Ps：指令是 vue 为开发者提供的模板语法，用来辅助开发者渲染页面的结构。
  - 美化样式
  基础 CSS 样式，美化网页的可视化效果。
  - 处理交互
  基于 vue 中提供的**事件绑定**，可以轻松处理用户和页面之间的交互行为。Ps：开发者把工作的重心放在核心业务的实现上

## 框架
官方给 vue 的定位是前端框架，因为它提供了构建用户界面的一整套解决方案（俗称 vue 全家桶）：
- vue（核心库）
- vue-router（路由方案）
- vuex（状态管理方案）
- vue 组件库（快速搭建页面 UI 效果的方案）

以及辅助 vue 项目开发的一系列工具：
- vue-cli（npm 全局包：一键生成工程化的 vue 项目 - 基于 webpack、大而全）
- vite（npm 全局包：一键生成工程化的 vue 项目 - 小而巧）
- vue-devtools（浏览器插件：辅助调试的工具）
- vetur（vscode 插件：提供语法高亮和智能提示）

## 特性
### 1. 数据驱动视图
vue会监听数据的变化，然后自动渲染
> 数据驱动视图时**单向的数据绑定**

### 2. 双向数据绑定
在**填写表单**时，双向数据绑定可以辅助开发者在**不操作 DOM 的前提下**，**自动**把用户填写的内容同步到数据源中。
> 开发者不再需要手动操作 DOM 元素，来获取表单元素最新的值！

## MVVM

在 MVVM 概念中：
**M**odel 表示当前页面渲染时所依赖的数据源。
->
**V**iew   表示当前页面所渲染的 DOM 结构。
->
**V**iew**M**odel 表示 vue 的实例，它是 MVVM 的核心。
```
      ->  DOM Listerners ->
view  <-  Data Bindings  <-  Model
 |             |               |
 V             V               V
DOM         vue实例          JS对象
```
ViewModel 作为 MVVM 的核心，是它把当前页面的数据源（Model）和页面的结构（View）连接在了一起。当数据源发生变化时，会被 ViewModel 监听到，VM 会根据最新的数据源自动更新页面的结构当表单元素的值发生变化时，也会被 VM 监听到，VM 会把变化过后最新的值自动同步到 Model 数据源中

# 通过代码了解vue
## 基本用法
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- 2. 声明要被 vue 所控制的 DOM 区域 -->
    <div id="app">{{age}}</div>

    <!-- 1. 导入 vue 的脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>

    <!-- 3. 创建 vue 的实例对象 -->
    <script>
      const vm = new Vue({
        // 3.1 使用 el 属性，指定 vue 要控制的区域
        el: '#app',
        // 3.2 数据源
        data: {
          username: 'zs',
          age: 20
        },
      })
    </script>
  </body>
</html>
```

## 内容渲染指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <p v-text="username"></p>

    <!-- 这里会被覆盖 -->
    <p v-text="gender">性别</p> 

    <!-- 更推荐使用下面这个 -->
    <p>姓名：{{username}}</p>
    <p>性别：{{gender}}</p>

    <p>---------</p>

    <p v-text="desc"></p>
    <p>{{desc}}</p>
    <!-- -text 指令和插值表达式只能渲染纯文本内容。如果要把包含 HTML 标签的字符串渲染为页面的 HTML 元素，则需要用到 v-html 这个指令： -->
    <p v-html="desc"></p>
  </div>
  
  <script src="./lib/vue-2.6.12.js"></script>

  <script>
    const vm = new Vue({
      el: '#app',
      data: {
        username: 'zs',
        gender: '男',
        desc: '<i style="color:red;">abc<i>'
      }
    })
  </script>
</body>
</html>
```

## 属性绑定指定
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <!-- :是v-bind的简单写法 -->
    <div id="app">
      <input type="text" :placeholder="inputValue">
      <hr>
      <img :src="imgSrc" alt="">
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 文本框的占位符内容
          inputValue: '请输入内容',
          // 图片的 src 地址
          imgSrc: './images/logo.png',
        },
      })
    </script>
  </body>
</html>
```

## 使用js表达式
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <div id="app">
      <p>{{number + 1}}</p>
      <p>{{ok ? 'True' : 'False'}}</p>
      <p>{{message.split('').reverse().join('')}}</p>
      <p :id="'list-' + id">xxx</p>
      <p>{{user.name}}</p>
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 数值
          number: 9,
          // 布尔值
          ok: false,
          // 字符串
          message: 'ABC',
          // id 值
          id: 3,
          // 用户的信息对象
          user: {
            name: 'zs',
          },
        },
      })
    </script>
  </body>
</html>

```

## 事件绑定指令
vue 提供了 v-on 事件绑定指令，用来辅助程序员为 DOM 元素绑定事件监听。

注意：原生 DOM 对象有 **onclick、oninput、onkeyup** 等原生事件，替换为 vue 的事件绑定形式后，
分别为：**v-on:click、v-on:input、v-on:keyup**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <div id="app">
      <h3>count 的值为：{{count}}</h3>
      <!-- TODO：点击按钮，让 data 中的 count 值自增 +1 -->
      <button v-on:click="addCount">+1</button>
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 计数器的值
          count: 0,
        },
        methods: {
          // 点击按钮，让 count 自增 +1
          // 这里的this指向的是vue实例
          addCount() {
            this.count += 1
          },
        },
      })
    </script>
  </body>
</html>
```
## 事件绑定指令的简写形式

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <div id="app">
      <h3>count 的值为：{{count}}</h3>
      <!-- TODO：点击按钮，让 data 中的 count 值自增 +1 -->
      <button @click="count+=1">+1</button>
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 计数器的值
          count: 0,
        },
        methods: {
          // 点击按钮，让 count 自增 +1
          // addCount() {
          //   this.count += 1
          // },
        },
      })
    </script>
  </body>
</html>
```

## 事件对象event

在原生的 DOM 事件绑定中，可以在事件处理函数的形参处，接收事件对象 event。同理，在 v-on 指令（简写为 @ ）所绑定的事件处理函数中，同样可以接收到事件对象 event

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <div id="app">
      <h3>count 的值为：{{count}}</h3>
      <button v-on:click="addCount">+1</button>
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 计数器的值
          count: 0,
        },
        methods: {
          // 点击按钮，让 count 自增 +1
          addCount(e) {
            const nowBgColor = e.target.style.backgroundColor
            e.target.style.backgroundColor = nowBgColor === 'red' ? '' : 'red'
            this.count += 1
          },
        },
      })
    </script>
  </body>
</html>

```

## 绑定事件并传参
在使用 v-on 指令绑定事件时，可以使用 ( ) 进行传参
$event 是 vue 提供的特殊变量，用来表示原生的事件参数对象 event。$event 可以解决事件参数对象 event 
被覆盖的问题。
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- vue 实例要控制的 DOM 区域 -->
    <div id="app">
      <h3>count 的值为：{{count}}</h3>
      <button @click="addCount(2, $event)">+2</button>
    </div>

    <!-- 导入 vue 脚本文件 -->
    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 创建 VM 实例对象
      const vm = new Vue({
        // 指定当前 VM 要控制的区域
        el: '#app',
        // 数据源
        data: {
          // 计数器的值
          count: 0,
        },
        methods: {
          addCount(step, e) {
            const bgColor = e.target.style.backgroundColor
            e.target.style.backgroundColor = bgColor === 'red' ? '' : 'red'
            this.count += step
          },
        },
      })
    </script>
  </body>
</html>

```
## 事件修饰符
在事件处理函数中调用 preventDefault() 或 stopPropagation() 是非常常见的需求。因此，vue 提供了事件修饰符的概念，来辅助程序员更方便的对事件的触发进行控制。常用的 5 个事件修饰符如下：
|事件修饰符|说明|
|---------|----|
|**.prevent**|**阻止默认行为（例如：阻止 a 连接的跳转、阻止表单的提交等）preventDefault()**|
|**.stop**|**阻止事件冒泡 stopPropagation()**|
|.capture|以捕获模式触发当前的事件处理函数|
|.once|绑定的事件只触发1次|
|.self|只有在 event.target 是当前元素自身时触发事件处理函数|


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .inner {
      line-height: 100px;
      background-color: aliceblue;
      font-size: 13px;
      text-align: center;
    }

    .outer {
      background-color: bisque;
      padding: 50px;
      font-size: 13px;
    }

    .box {
      background-color: coral;
      padding: 50px;
    }
  </style>
</head>

<body>
  <!-- 在页面中声明一个将要被 vue 所控制的 DOM 区域 -->
  <div id="app">
    <h4>① .prevent 事件修饰符的应用场景</h4>
    <a href="https://www.baidu.com" @click.prevent="onLinkClick">百度首页</a>

    <hr />

    <h4>② .stop 事件修饰符的应用场景</h4>
    <div class="outer" @click="onOuterClick">
      外层的 div
      <div class="inner" @click.stop="onInnerClick">内部的 div</div>
    </div>

    <hr />

    <h4>③ .capture 事件修饰符的应用场景</h4>
    <div class="outer" @click.capture="onOuterClick">
      外层的 div
      <div class="inner" @click="onInnerClick">内部的 div</div>
    </div>

    <hr />

    <h4>④ .once 事件修饰符的应用场景</h4>
    <div class="inner" @click.once="onInnerClick">内部的 div</div>

    <hr />

    <h4>⑤ .self 事件修饰符的应用场景</h4>
    <div class="box" @click="onBoxClick">
      最外层的 box
      <div class="outer" @click.self="onOuterClick">
        中间的 div
        <div class="inner" @click="onInnerClick">内部的 div</div>
      </div>
    </div>

    <hr />
  </div>

  <script src="./lib/vue-2.6.12.js"></script>
  <script>
    const vm = new Vue({
      el: '#app',
      // 声明处理函数的节点
      methods: {
        // 超链接的点击事件处理函数
        onLinkClick() {
          alert('ok')
        },
        // 点击了外层的 div
        onOuterClick() {
          console.log('触发了 outer 的 click 事件处理函数')
        },
        // 点击了内部的 div
        onInnerClick() {
          console.log('触发了 inner 的 click 事件处理函数')
        },
        onBoxClick() {
          console.log('触发了 box 的 click 事件处理函数')
        }
      },
    })
  </script>
</body>

</html>
```

## 按键修饰符
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <input type="text" @keyup.enter="submit" @keyup.esc="clearInput" />
    </div>

    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        data: {},
        methods: {
          // 获取文本框最新的值
          submit(e) {
            console.log('摁下了 enter 键，最新的值是：' + e.target.value)
          },
          // 清空文本框的值
          clearInput(e) {
            e.target.value = ''
          },
        },
      })
    </script>
  </body>
</html>

```

## 双向绑定指令
vue 提供了 v-model 双向数据绑定指令，用来辅助开发者在不操作 DOM 的前提下，快速获取表单的数据。

**v-model 指令只能配合表单元素一起使用.**
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p>用户名是：{{username}}</p>
      <input type="text" v-model="username" />

      <hr />

      <p>选中的省份是：{{province}}</p>
      <select v-model="province">
        <option value="">请选择</option>
        <option value="1">北京</option>
        <option value="2">河北</option>
        <option value="3">黑龙江</option>
      </select>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          // 姓名
          username: 'zs',
          // 省份
          province: '1',
        },
      })
    </script>
  </body>
</html>

```

## v-mode指令的修饰符
为了方便对用户输入的内容进行处理，vue 为 v-model 指令提供了 3 个修饰符，分别是：
|修饰符|作用|示例|
|----|----|----|
|.number|自动将用户的输入值转为数值类型|`<input v-model.number="age" />`|
|.trim|自动过滤用户输入的首尾空白字符 |`<input v-model.trim="msg" />`|
|.lazy|在“change”时而非“input”时更新 |`<input v-model.lazy="msg" />`|

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      姓名：<input type="text" v-model.trim="username" />

      <hr />

      年龄：<input type="text" v-model.number="age" />

      <hr />

      地址：<input type="text" v-model.lazy="address" />
    </div>

    <script src="./lib/vue-2.6.12.js"></script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          // 姓名
          username: 'zs',
          // 年龄
          age: 1,
          // 地址
          address: '北京市',
        },
      })
    </script>
  </body>
</html>

```

## 条件渲染指令
v-if 和 v-show 的区别:

实现原理不同：
- v-if 指令会动态地创建或移除 DOM 元素，从而控制元素在页面上的显示与隐藏；
- v-show 指令会动态为元素添加或移除 style="display: none;" 样式，从而控制元素的显示与隐藏；


性能消耗不同：
v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
- 如果需要非常频繁地切换，则使用 v-show 较好
- 如果在运行时条件很少改变，则使用 v-if 较好

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <button @click="flag = !flag">Toggle Flag</button>

      <p v-if="flag">请求成功 --- 被 v-if 控制</p>
      <p v-show="flag">请求成功 --- 被 v-show 控制</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          // flag 用来控制元素的显示与隐藏
          // 值为 true 时显示元素
          // 值为 false 时隐藏元素
          flag: false,
        },
      })
    </script>
  </body>
</html>

```

## v-else和v-else-if指令
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p v-if="num > 0.5">随机数 ＞ 0.5</p>
      <p v-else>随机数 ≤ 0.5</p>

      <hr />

      <p v-if="type === 'A'">优秀</p>
      <p v-else-if="type === 'B'">良好</p>
      <p v-else-if="type === 'C'">一般</p>
      <p v-else>差</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          // 生成 1 以内的随机数
          num: Math.random(),
          // 类型
          type: 'A'
        },
      })
    </script>
  </body>
</html>

```

## 列表渲染指令
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <ul>
        <li v-for="(user, i) in list">索引是：{{i}}，姓名是：{{user.name}}</li>
      </ul>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          // 用户列表的数据
          list: [
            { id: 1, name: 'zs' },
            { id: 2, name: 'ls' },
          ],
        },
      })
    </script>
  </body>
</html>

```
## v-for中的key
当列表的数据变化时，默认情况下，vue 会尽可能的复用已存在的 DOM 元素，从而提升渲染的性能。但这种默认的性能优化策略，会导致有状态的列表无法被正确更新。

为了给 vue 一个提示，以便它能跟踪每个节点的身份，从而在保证有状态的列表被正确更新的前提下，提升渲染的性能。此时，需要为每项提供一个唯一的 key 属性。

key 的注意事项
1. key 的值只能是字符串或数字类型
2. key 的值必须具有唯一性（即：key 的值不能重复）
3. 建议把数据项 id 属性的值作为 key 的值（因为 id 属性的值具有唯一性）
4. 使用 index 的值当作 key 的值没有任何意义（因为 index 的值不具有唯一性）
5. 建议使用 v-for 指令时一定要指定 key 的值（既提升性能、又防止列表状态紊乱）

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <!-- 在页面中声明一个将要被 vue 所控制的 DOM 区域 -->
  <div id="app">

    <!-- 添加用户的区域 -->
    <div>
      <input type="text" v-model="name">
      <button @click="addNewUser">添加</button>
    </div>

    <!-- 用户列表区域 -->
    <ul>
      <li v-for="(user, index) in userlist" :key="user.id">
        <input type="checkbox" />
        姓名：{{user.name}}
      </li>
    </ul>
  </div>

  <script src="./lib/vue-2.6.12.js"></script>
  <script>
    const vm = new Vue({
      el: '#app',
      data: {
        // 用户列表
        userlist: [
          { id: 1, name: 'zs' },
          { id: 2, name: 'ls' }
        ],
        // 输入的用户名
        name: '',
        // 下一个可用的 id 值
        nextId: 3
      },
      methods: {
        // 点击了添加按钮
        addNewUser() {
          this.userlist.unshift({ id: this.nextId, name: this.name })
          this.name = ''
          this.nextId++
        }
      },
    })
  </script>
</body>

</html>
```
## 过滤器的基本使用
过滤器（Filters）常用于文本的格式化。例如：
hello -> Hello

过滤器应该被添加在 JavaScript 表达式的尾部，由“**管道符“|”**”进行调用。

过滤器可以用在两个地方：插值表达式
和 v-bind 属性绑定
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
    <!-- 这里 -->
      <p :title="info | capitalize">{{message | capitalize}}</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          message: 'hello vue.js',
          info: 'title info',
        },
        filters: {
          capitalize(str) {
            return str.charAt(0).toUpperCase() + str.slice(1)
          }
        }
      })
    </script>
  </body>
</html>

```
## 私有过滤器和全局过滤器
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p :title="info | capitalize">{{message | capitalize}}</p>
    </div>

    <div id="app2">
      <p>{{abc | capitalize}}</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 全局过滤器
      Vue.filter('capitalize', (str) => {
        return str.charAt(0).toUpperCase() + str.slice(1) + '~~~'
      })
    </script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          message: 'hello vue.js',
          info: 'title info',
        },
        // 私有过滤器，只能被当前 vm 所控制的区域所使用
        filters: {
          capitalize(str) {
            return str.charAt(0).toUpperCase() + str.slice(1)
          },
        },
      })
    </script>

    <script>
      const vm2 = new Vue({
        el: '#app2',
        data: {
          abc: 'abc'
        }
      })
    </script>
  </body>
</html>

```
## 连续调用多个过滤器
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p :title="info | capitalize">{{message | capitalize | maxLength}}</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 全局过滤器
      // 首字母转大写的过滤器
      Vue.filter('capitalize', (str) => {
        return str.charAt(0).toUpperCase() + str.slice(1)
      })

      // 定义控制文本长度的过滤器
      Vue.filter('maxLength', (str) => {
        if(str.length <= 10) return str
        return str.slice(0, 10) + '...'
      })
    </script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          message: 'hello vue.js',
          info: 'title info',
        },
      })
    </script>
  </body>
</html>

```

## 过滤器传参

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <p :title="info | capitalize">{{message | capitalize | maxLength(3)}}</p>
    </div>

    <script src="./lib/vue-2.6.12.js"></script>
    <script>
      // 全局过滤器
      // 首字母转大写的过滤器
      Vue.filter('capitalize', (str) => {
        return str.charAt(0).toUpperCase() + str.slice(1)
      })

      // 定义控制文本长度的过滤器
      Vue.filter('maxLength', (str, len = 10) => {
        if(str.length <= len) return str
        return str.slice(0, len) + '...'
      })
    </script>

    <script>
      const vm = new Vue({
        el: '#app',
        data: {
          message: 'hello vue.js',
          info: 'title info',
        },
      })
    </script>
  </body>
</html>

```

