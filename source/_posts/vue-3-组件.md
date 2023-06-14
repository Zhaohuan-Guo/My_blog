---
title: Vue-3-组件
date: 2023-06-04 19:00:18
tags:
    - vue
    - JavaScript
categories: 
    - vue
cover: /images/vue_cover.png
---
# 单页面运用程序
SPA 单页面应用程序最显著的 3 个优点如下：
1. 良好的交互体验
   - 单页应用的内容的改变不需要重新加载整个页面
   - 获取数据也是通过 Ajax 异步获取
   - 没有页面之间的跳转，不会出现“白屏现象”
2. 良好的前后端工作分离模式
   - 后端专注于提供 API 接口，更易实现 API 接口的复用
   - 前端专注于页面的渲染，更利于前端工程化的发展  
3. 减轻服务器的压力
   - 服务器只提供数据，不负责页面的合成与逻辑的处理，吞吐能力会提高几倍

任何一种技术都有自己的局限性，对于 SPA 单页面应用程序来说，主要的缺点有如下两个：
1. 首屏加载慢
   - 路由懒加载
   - 代码压缩
   - CDN 加速
   - 网络传输压缩
2. 不利于 SEO
   - SSR 服务器端渲染
> **SSR（Server-Side Rendering）**指的是服务器端渲染，它是一种在服务器端生成完整的 HTML 页面并将其发送给客户端的技术。相对于传统的前端渲染方式（客户端渲染），SSR 的主要特点是在服务器端进行页面的渲染和数据的获取，然后将渲染好的 HTML 页面发送给客户端。
**在 SSR 中**，服务器端负责处理路由和数据获取，执行页面的渲染逻辑，并将生成的 HTML 页面返回给客户端。客户端接收到HTML 页面后，可以直接展示给用户，不需要再等待JavaScript 的加载和执行。这样可以提供更快的首次渲染速度，改善了网页的加载性能和用户体验。
**SSR 的主要优点**是对于搜索引擎优化（SEO）友好。由于搜索引擎爬虫在抓取网页时更容易获取和解析服务器端渲染的 HTML 内，因此可以更好地理解和索引网页的内容。相比之下，客户端渲染的网页通常需要等待 JavaScript 加载和执行后才能生成完整的 HTML，这可能导致搜索引擎无法正确抓取和索引页面的内容，影响网页的搜索排名和可搜索性。
**SSR 的实现方式有多种**，可以使用后端框架如Next.js（基于React）或Nuxt.js（基于Vue.js），它们提供了便捷的服务器端渲染功能。在使用 SSR 时，需要注意服务器的性能和扩展性，以及在服务器和客户端之间的数据同步和状态管理等方面的问题。
**需要注意的是**，SSR 并不是适用于所有情况的最佳解决方案。对于一些复杂的交互逻辑和动态内容更新频繁的应用程序，客户端渲染可能更合适。因此，在选择前端渲染方式时，需要根据具体的需求和场景来进行权衡和选择。

## 创建项目
vue 官方提供了两种快速创建工程化的 SPA 项目的方式：
1. 基于 vite 创建 SPA 项目
2. 基于 vue-cli 创建 SPA 项目

使用 vite 创建的项目结构如下：
```
│   .gitignore
│   index.html
│   package-lock.json
│   package.json
├───public
│       favicon.ico
└───src
    │   App.vue
    │   index.css
    │   main.js
    ├───assets
    │       logo.png
    └───components
        │   App.vue
        ├───01.globalReg
        │       Swiper.vue
        │       Test.vue
        ├───02.privateReg
        │       Search.vue
        ├───03.style
        │       App.vue
        │       List.vue
        ├───04.props
        │       App.vue
        │       Article.vue
        ├───05.class&style
        │       App.vue
        │       Style.vue
        └───06.MyHeader
                MyHeader.vue

```
其中：
- node_modules 目录用来存放第三方依赖包
- public 是公共的静态资源目录
- src 是项目的源代码目录（程序员写的所有代码都要放在此目录下）
- .gitignore 是 Git 的忽略文件
- index.html 是 SPA 单页面应用程序中唯一的 HTML 页面
- package.json 是项目的包管理配置文件
- assets 目录用来存放项目中所有的静态资源文件（css、fonts等）
- components 目录用来存放项目中所有的自定义组件
- App.vue 是项目的根组件
- index.css 是项目的全局样式表文件
- main.js 是整个项目的打包入口文件


在工程化的项目中，vue 要做的事情很单纯：通过 main.js 把 App.vue 渲染到 index.html 的指定区域中。
其中：
1. **App.vue** 用来编写待渲染的模板结构
2. **index.html** 中需要预留一个 el 区域
3. **main.js** 把 App.vue 渲染到了 index.html 所预留的区域中

# vue 组件的构成
## vue 组件组成结构
每个 .vue 组件都由 3 部分构成，分别是：
- **template** -> 组件的模板结构
- **script** -> 组件的 JavaScript 行为
- **style** -> 组件的样式
其中，每个组件中必须包含 template 模板结构，而 script 行为和 style 样式是可选的组成部分


### 组件的 script 节点
```vue
<script>
// 组件相关的data数据，methods方法等，都需要定义到 export default所导出的对象中
export default {
   name: 'MyApp',
   data() { 
      return {
         count: 0,
      }
   },
   method: {
      addCount() {
         this.count++
      }
   },
}
</script>
```

### 组件的 style 节点
```vue
<style lang = 'less'>
h1 {
   font-weight: normal;
   i {
      color: red;
      font-style: normal;
   }
}
</style>
```

# 组件的使用
## 组件的注册
vue 中组件的引用原则：**先注册后使用**

vue 中注册组件的方式分为“全局注册”和“局部注册”两种，其中：
- 被全局注册的组件，**可以在全局任何一个组件内使用**
```js
import {createApp} from 'vue'
import App from './App.vue'

//导入组件
import Swiper from './components/MySwiper.vue'
import Test from './components/MyTest.vue'

const app = creatApp(App)

//全局注册组件
app.component('my-swiper', Swiper)
app.component('my-test', Test)

app.mount('#app')
```
使用 app.component() 方法注册的全局组件，直接以标签的形式进行使用即可，例如:
```js
<template>
   <h1>这是App 根组件</h1>
   <hr/>
   <my-swiper></my-swiper>
   <my-test></my-test>
</template>
```

- 被局部注册的组件，**只能在当前注册的范围内使用**
```js
<template>
   <h1>这是App 根组件</h1>
   <hr/>
   <my-swiper></my-swiper>
   <my-test></my-test>
</template>

<script>
   import Search from './components/MySearch.vue'
   export default {
      components {
         'my-search': Search,
      }
   }
</script>
```

## 通过 name 属性注册组件

*在实际开发中，推荐使用**帕斯卡命名法（驼峰命名法）**为组件注册名称，因为它的适用性更强*

在注册组件期间，除了可以直接提供组件的注册名称之外，还可以把组件的 name 属性作为注册后组件的名称

```VUE
<template>
   <h3>this is a component<h3>
</template>

<script>
export default {
   name: 'MySwiper'
}
</script>
```

```js
import Swiper from './components/MySwiper.vue'
app.components(Swiper.name, Swiper)
```
## 组件之间的样式冲突问题
默认情况下，写在 .vue 组件中的样式会全局生效，因此很容易造成多个组件之间的样式冲突问题。导致组件之间样式冲突的根本原因是：
1. 单页面应用程序中，所有组件的 DOM 结构，都是基于**唯一的 index.html** 页面进行呈现的
2. 每个组件中的样式，都会**影响整个 index.html 页面**中的 DOM 元素

### 解决方案

为每个组件分配唯一的自定义属性，在编写组件样式时，通过属性选择器来控制样式的作用域：
```vue
<template>
   <div class="container" data-v-001>
      <h3 data-v-001>轮播图组件</h3>
   </div>
</template>

<style>
   .container[data-v-001] {
      border: 1px solid red;
   }
</style>
```
为了提高开发效率和开发体验，vue 为 style 节点提供了 scoped 属性，从而防止组件之间的样式冲突问题

```vue
<template>
   <div class="container">
      <h3>轮播图组件</h3>
   </div>
</template>

<style scoped>
   .container{
      border: 1px solid red;
   }
</style>
```

### /deep/ 样式穿透
如果给当前组件的 style 节点添加了 scoped 属性，则当前组件的样式对其子组件是不生效的。如果想让某些样式对子组件生效，**可以使用 /deep/ 深度选择器**。
```vue
<style lang='less' scoped>
.title {
   color: blue;
}

:deep() .title{
   color: blue;
}
</style>
```
**注意：/deep/ 是 vue2.x 中实现样式穿透的方案。在 vue3.x 中推荐使用 :deep() 替代 /deep/。**


## props
props 是组件的自定义属性，组件的使用者可以通过 props 把数据传递到子组件内部，供子组件内部进行使用。
