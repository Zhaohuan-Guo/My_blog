---
title: front
date: 2023-05-15 23:56:11
tags:
    - Front end
    - CSS
    - JS
    - HTML
    - Vue
categorie: Knowledge point
cover: https://img1.baidu.com/it/u=1848353671,3362559478&fm=253&fmt=auto&app=138&f=PNG?w=853&h=500
---
# HTML
## html的语义化标签
准确地描述网页内容的结构和含义。这些语义化标签有助于提高网页的可读性、可访问性以及搜索引擎优化。
## HTML5/CSS3/ES6，3，5，6什么意思？
分别指的是他们各自的版本，HTML是创建网页用的标记语言，是第五个版本，增加了一些特性，使得开发者可以更好地处理多媒体内容、绘图、动画和交互性方面。

- 多媒体内容：引入了```<audio>```, ```<video>```，用于嵌入音视频。
    - ```<track>```
    - play, pause, ended等事件
- 画布```<canvas>```
- SVG：可以无损地放大缩小而不失真。
- WebRTC
- CSS3动画和过渡

CSS3（Cascading Style Sheets 3）：CSS用于为HTML文档添加样式和布局。CSS3是CSS的第三个主要版本，引入了许多新的特性，如圆角、阴影、渐变、动画和过渡效果等。CSS3使得开发者可以更好地控制网页的外观和交互效果。

- border-radius
- box-shadow
- linear-gradient
- @keyframes
- animation
```css
    @keyframes myAnimation {
  0% {
    /* 起始关键帧的样式 */
  }
  50% {
    /* 中间关键帧的样式 */
  }
  100% {
    /* 结束关键帧的样式 */
  }
}
```
- transition
- media queries

ES6（ECMAScript 6，也被称为ES2015）：ECMAScript是JavaScript的标准规范，ES6是ECMAScript的第六个版本。ES6引入了许多新的语法和功能，包括箭头函数、模板字符串、解构赋值、类和模块等。ES6的出现使得JavaScript语言更加强大和灵活，并提供了更好的开发工具和模式。
## 上述三个东西已经用了快十年了，会涉及到老版本，关注到兼容性问题吗？
特殊情况下确实会存在兼容性问题：
1. CSS兼容性问题：
    - 不同浏览器对CSS属性和属性值的解析可能存在差异，导致样式在不同浏览器中显示不一致。
    - 浏览器厂商前缀（vendor prefix）的支持情况不同，需要添加不同的前缀以确保样式在各个浏览器中正常工作。
    - 旧版本的浏览器可能不支持某些CSS3的新特性，需要提供替代方案或使用Polyfills来实现兼容性。
2. JavaScript兼容性问题：
    - 不同浏览器对JavaScript语法和API的支持程度有所差异，可能导致代码在某些浏览器中不起作用或产生错误。
    - 旧版本浏览器可能不支持ES6及更高版本的语法和功能，需要使用Babel等工具进行转译或提供替代方案。
    - 不同浏览器对JavaScript的性能和优化支持程度不同，可能导致代码在性能较低的浏览器中运行缓慢或产生问题。
3. HTML兼容性问题：
    - 不同浏览器对HTML标准的解析和渲染可能存在差异，导致页面在不同浏览器中显示不一致。
    - 旧版本浏览器可能不支持HTML5新增的元素和功能，需要提供替代方案或使用Polyfills来实现兼容性。
    - 响应式设计和移动设备兼容性问题：
    - 不同设备和屏幕尺寸可能对响应式设计的支持程度有所差异，需要进行适配和测试以确保在各种设备上呈现良好。
    - 旧版本的移动浏览器可能对CSS媒体查询和触摸事件的支持不完善，需要提供备用方案或使用Polyfills。
4. 图片和多媒体兼容性问题：
    - 不同浏览器对图片格式（如JPEG、PNG、WebP等）和视频格式（如MP4、WebM等）的支持程度不同，需要提供多种格式的备选方案。
    - 旧版本浏览器可能不支持某些HTML5的多媒体元素和功能，需要提供替代方案或使用Polyfills来实现兼容性。
一些关于解决方案的设计理念：
1. 特性检测（Feature Detection）：在使用HTML5、CSS3和ES6的新特性之前，可以进行特性检测来确定当前环境是否支持该特性。通过检测特性的存在与否，你可以选择使用备用方案或提供降级的功能。
2. 渐进增强（Progressive Enhancement）：渐进增强是一种设计理念，它从基本的功能开始构建，然后逐渐添加更高级的功能和效果。通过这种方式，你可以确保网站在不支持某些高级功能的旧浏览器上仍然可以正常使用，而在支持这些功能的现代浏览器上则提供更丰富的体验。
3. 浏览器兼容性表格（Browser Compatibility Tables）：在开发过程中，可以使用浏览器兼容性表格来查看特定特性在不同浏览器中的支持情况。这些表格提供了对不同浏览器版本和平台的支持信息，帮助你了解哪些特性需要额外的处理或备用方案。
## 介绍一下polyfills和babel
Polyfills和Babel是前端开发中常用的工具，用于处理浏览器兼容性和转换JavaScript代码的任务。它们有以下特点和功能：

Polyfills（垫片）：

- Polyfills是用于填充浏览器功能缺失的代码或脚本库。
它们提供了对新的Web标准、API或功能的模拟实现，以便让旧版本浏览器能够支持现代的Web技术。
- Polyfills通常用于实现HTML5、CSS3、ES6+等新特性在旧版本浏览器中的兼容性。
- 开发者可以选择和引入特定的Polyfills来满足项目的需求，以确保代码在各种浏览器中正常工作。
- 一些流行的Polyfills库包括Polyfill.io、Modernizr和Babel。

Babel：

- Babel是一个JavaScript编译器，用于将使用较新版本的JavaScript语法和特性编写的代码转换为向后兼容的代码。
- 它允许开发者使用最新的JavaScript语言特性，而不必担心代码在旧版本浏览器中无法运行。
- Babel的工作原理是通过解析、转换和生成三个阶段来处理JavaScript代码。
- 开发者可以通过插件和预设来配置Babel，选择需要的转换功能和目标环境。
- Babel还支持将模块语法转换为浏览器可识别的格式，以实现模块化开发和加载。
## meta 标签是什么，做什么用的？
叫做元标签，提供网页元数据的相关信息。顾名思义，是描述数据的数据们提供了网也作者、内容、关键词、字符编码等信息。
## meta标签的viewport标签
用于控制移动设备上网页的视口，匹配设备宽度，然后进行缩放
# CSS
## css 里面的单位
1. px:根据屏幕分辨率进行计算。
2. %： 根据父类元素进行计算。
3. em: 相对于元素自身的字体尺寸进行计算。
4. rem: 相对于根元素，更具有可控性。
5. vh和vw: 根据视窗。
6. ch:字符'0'的宽度
## 两个 div 横向的排列
1. 使用浮动（float）
浮动的原理：
浮动元素会脱离正常的文档流，向指定的方向靠，直到遇到父元素或者其他浮动元素的边界。
坏处：导致父元素的高度塌陷，父元素无法计算其正常高度（包裹不住浮动元素）。
解决办法：使用clearfix：通过在父元素中添加一个额外的空元素，使其成为一个浮动元素的兄弟元素，并应用清除浮动的样式规则。
```html
<style>
  .clearfix::after {
    content: "";
    display: table;
    clear: both;
  }
</style>

<div class="parent clearfix">
  <div class="float-left">浮动元素 1</div>
  <div class="float-left">浮动元素 2</div>
</div>
```
```html
<div style="float: left;">左侧内容</div>
<div style="float: left;">右侧内容</div>
```
2. 使用弹性盒子布局（flexbox）
原理：
构造一个弹性容器。分为主轴和交叉轴，主轴水平方向，交叉轴垂直方向。
flex-direction: 决定排列方向
justify-content：主轴对齐方式
align-items：交叉轴对齐方式
```html
<div style="display: flex;">
  <div>左侧内容</div>
  <div>右侧内容</div>
</div>
```
## 伪元素介绍：
用```::``` 表示，是一种特殊选择器。
优点：不占用DOM节点，简化开发
## 移动端/PC端 不同分辨率怎么适配？
1. 响应式设计优先
    1. 媒体查询
    2. 流式布局-相对单位
    3. 弹性盒子
    4. 图片适配
    5. 栅格布局
2. 定义布局需求
3. 图片和媒体
## css选择器权重
- inline styles-最高优先级
- ID: 100
- . [] : 10
- 伪元素选择器 1


# JS
## JS有哪些基础数据类型
1. Number
2. String
3. Boolean
4. Null
5. Undefined: 声明了但是没有赋值
6. Symbol: ES6新引进的数据类型，表示独一无二的值
    - 括号里面的值只是为了方便调试，无实际意义。
    - 使用场景：    
        1. 创建唯一的属性键 
        2. 隐藏对象的特定属性
            封装性和私有性
        3. 在迭代器和生成器中使用
```javascript
// 创建唯一的属性键
const id = Symbol("id");

const obj = {
  [id]: 12345,
};

console.log(obj[id]); // 输出 12345

//隐藏对象的特定属性
const _firstName = Symbol("firstName");
const _lastName = Symbol("lastName");

class Person {
  constructor(firstName, lastName) {
    this[_firstName] = firstName;
    this[_lastName] = lastName;
  }

  getFullName() {
    return `${this[_firstName]} ${this[_lastName]}`;
  }
}

const person = new Person("John", "Doe");
console.log(person.getFullName()); // 输出 "John Doe"
console.log(person[_firstName]); // undefined，外部无法访问私有属性

//在迭代器和生成器中使用
const myIterable = {
  [Symbol.iterator]() {
    let count = 1;

    return {
      next() {
        if (count <= 5) {
          return { value: count++, done: false };
        } else {
          return { done: true };
        }
      },
    };
  },
};

for (const num of myIterable) {
  console.log(num);
}
```
## next 方法是什么
next方法是一个生成器函数的方法。生成器函数是一种特殊类型的函数，可以在需要的时候生成一系列的值。
每次调用next（）会返回一个包含value和done属性的对象。
## 如何声明
1. var 声明一个变量并赋值，函数作用域
2. let 声明一个块级作用域的变量，即所在的那一个大括号
3. const 声明一个块级作用域的常量
## 深拷贝的实现方法
1. 使用第三方库：许多 JavaScript 第三方库提供了深拷贝功能，如 Lodash 的 ```_.cloneDeep()``` 方法。这些库通常有更复杂和全面的实现，可以处理各种特殊情况。
2. JSON 序列化和反序列化：利用 JSON.stringify() 将对象转为字符串，再用 JSON.parse() 将字符串转回对象。这种方法适用于大多数简单对象和数组，但无法处理函数和特殊对象。
3. 逐个拷贝
```javascript
function deepClone(obj, visited = new WeakMap()) {
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }

  // 检查是否已经拷贝过该对象，避免循环引用
  if (visited.has(obj)) {
    return visited.get(obj);
  }

  let clone = Array.isArray(obj) ? [] : {};

  // 将当前对象添加到已拷贝对象的集合中
  visited.set(obj, clone);

  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      clone[key] = deepClone(obj[key], visited);
    }
  }

  return clone;
}
```
## forEach和map有什么区别

1. 使用方式：

forEach：对数组中的每个元素执行提供的回调函数，没有返回值。
map：对数组中的每个元素执行提供的回调函数，并返回一个新的数组，新数组的元素由回调函数的返回值组成。

2. 返回值：

forEach：没有返回值，只是用于遍历数组，执行回调函数。
map：返回一个新的数组，该数组的元素是由回调函数的返回值组成。

3. 副作用：

forEach：可以在回调函数中对原数组进行修改，会对原数组产生副作用。
map：不会对原数组进行修改，它是将回调函数的返回值组成一个新的数组返回。

## 异步js依赖什么机制和技术
1. 回调函数
回调函数是一种常见的处理异步操作的方式，通过将一个函数作为参数传递给另一个函数，在异步操作完成后调用该函数来处理结果。
```javascript
function fetchData(url, callback) {
  // 模拟异步操作，例如发送网络请求
  setTimeout(() => {
    const data = { name: 'John', age: 30 };
    callback(data); // 异步操作完成后调用回调函数，并传递结果
  }, 2000);
}

function handleData(data) {
  console.log(data);
}

fetchData('https://example.com/api', handleData);

```
2. Promise
Promise 是 JavaScript 提供的一种处理异步操作的机制。它表示一个异步操作的最终完成或失败，并且可以通过链式调用的方式进行操作。下面是一个使用 Promise 的示例：
```javascript
function fetchData(url) {
  return new Promise((resolve, reject) => {
    // 模拟异步操作，例如发送网络请求
    setTimeout(() => {
      const data = { name: 'John', age: 30 };
      resolve(data); // 异步操作成功时，调用 resolve 方法并传递结果
      // 或者在异步操作失败时，调用 reject 方法并传递错误信息
    }, 2000);
  });
}

fetchData('https://example.com/api')
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error(error);
  });

```
3. 异步函数（Async/await）：
```javascript
function fetchData(url) {
  return new Promise((resolve, reject) => {
    // 模拟异步操作，例如发送网络请求
    setTimeout(() => {
      const data = { name: 'John', age: 30 };
      resolve(data);
    }, 2000);
  });
}

async function getData() {
  try {
    const result = await fetchData('https://example.com/api');
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}

getData();

```

## 怎么给一个dom元素添加一个类
要给一个 DOM 元素添加一个类，可以使用 JavaScript 中的 classList 属性和相关方法。
- add(className)
- toggle(className)
- contains(className)

## const一个数组，是否能够push一个数据进去 为什么能或不能
能，这个数组被声明为const，它的引用不能被修改，但是数组本身可以被修改。
完全冻结可以用Object.freeze()
## pop呢，splice呢
不行，因为他们会返回一个新的数组就会报错
## Promise 有三种状态
1. Pending: 初始状态
2. Fulfilled: 操作完成 -> .then()
3. Rejected: 错误  -> .catch()
## promise一般用来做什么
Promise 一般用于处理异步操作和编写更清晰、可维护的异步代码。它提供了一种优雅的方式来处理异步任务的完成和错误处理。

具体来说，Promise 用于解决回调地狱（callback hell）问题，即当多个异步操作依赖于前一个异步操作的结果时，代码会变得混乱且难以理解。通过使用 Promise，可以通过链式调用 .then() 方法来按顺序执行异步操作，并使用 .catch() 方法捕获和处理错误，避免嵌套过深的回调函数。

> 回调地狱（callback hell）是指在异步编程中，多个异步操作依赖于前一个异步操作的结果，导致代码嵌套层级过深、难以理解和维护的情况。
在回调地狱中，由于每个异步操作都需要通过回调函数来处理结果或进行下一步操作，导致代码的缩进层级不断增加，代码变得难以阅读、理解和调试。嵌套过深的回调函数也增加了代码的复杂性和维护成本。```asyncOperation1(function(result1){ asyncOperation2(result1, function(result2) {asyncOperation3(result2, function(result3) {// ...});});});```
## 箭头函数和 function 区别
箭头函数的主要特点如下：

1. 简洁的语法：箭头函数可以用更短的语法来定义函数，省略了 function 关键字和大括号，使代码更加简洁易读。

2. 隐式返回：如果箭头函数的函数体只有一条语句，并且没有使用大括号 {} 包裹函数体，那么该语句将被隐式返回作为函数的返回值。

3. 改变了 this 的指向：箭头函数没有自己的 this，它会继承外层作用域的 this 值。这意味着箭头函数内部的 this 指向的是定义函数时的上下文，而不是函数被调用时的上下文。
# Vue
## 是什么
Vue.js 是一种用于构建用户界面的渐进式 JavaScript 框架
## 什么是渐进式框架
可以逐步采用它来构建项目，也可以将其集成到已有的项目中。你可以根据需求选择性地使用 Vue.js 的核心功能和插件。
## vue框架和别的框架有什么区别
1. 渐进式开发：Vue.js 是一个渐进式框架，意味着你可以逐步采用它来构建项目，也可以将其集成到已有的项目中。你可以根据需求选择性地使用 Vue.js 的核心功能和插件。
2. 双向数据绑定：Vue.js 支持双向数据绑定，即当数据发生变化时，视图会自动更新；同时，当用户与视图进行交互时，数据也会自动更新。这种数据绑定机制简化了开发过程，提高了开发效率。
3. 组件化开发：Vue.js 鼓励组件化开发，将一个页面拆分成多个独立可复用的组件。每个组件都包含自己的模板、样式和逻辑，使得代码结构清晰，易于维护和扩展。
4. 虚拟 DOM：Vue.js 使用虚拟 DOM 技术来优化性能。当数据发生变化时，Vue.js 会先生成一个虚拟 DOM，然后通过比较新旧虚拟 DOM 的差异，最终只更新需要更新的部分，而不是整个页面重新渲染。
5. 生态系统：Vue.js 拥有庞大的生态系统，包括官方维护的路由器（Vue Router）、状态管理工具（Vuex）、构建工具（Vue CLI）等，以及大量的第三方插件和库，可以满足各种开发需求。
## vue里的data如果改变了一个数据，是否能在view里显示出来
在 Vue 中，当 data 对象中的数据发生变化时，视图会自动更新以反映最新的数据。这是因为 Vue 使用了响应式系统来追踪数据的变化，并根据变化自动更新相关的视图。

1. 虚拟DOM：Vue.js通过创建一个虚拟DOM树来表示真实的DOM结构。当数据发生变化时，Vue.js会对比新旧虚拟DOM树的差异，并只更新需要变更的部分，而不是重新渲染整个视图。这种方式可以提高性能并减少DOM操作的开销。

2. 数据劫持：Vue.js使用了对象劫持的方式来追踪数据的变化。它通过使用ES5的Object.defineProperty方法来重写对象的访问器属性（getter和setter），从而在数据发生变化时能够触发相应的更新。当你修改data对象中的一个属性时，Vue.js会自动调用相应的setter方法，从而触发视图的更新。

## 虚拟 DOM 相对于真实 DOM 的缺陷
虚拟 DOM 相对于真实 DOM 的一些缺陷包括：

初始渲染耗时：使用虚拟 DOM 需要将真实 DOM 结构转换为虚拟 DOM 结构，然后进行比较和更新操作。这个过程可能会导致初始渲染的耗时增加。

内存占用增加：虚拟 DOM 需要在内存中维护一份虚拟 DOM 树，这意味着会增加一定的内存占用。对于大型应用或者频繁更新的情况，可能会占用较多的内存资源。

复杂度增加：使用虚拟 DOM 需要引入额外的代码和库来进行虚拟 DOM 的比较和更新操作。这增加了整体的代码复杂度，并且需要学习和理解虚拟 DOM 的工作原理和使用方式。

部分情况下性能下降：尽管虚拟 DOM 可以通过比较和更新操作减少真实 DOM 的操作次数，但在某些情况下，由于虚拟 DOM 需要额外的比较和计算，可能会导致性能下降，尤其是在简单的 UI 更新场景下。

尽管虚拟 DOM 有一些缺陷，但它仍然是一种强大的工具，能够简化复杂的 UI 更新逻辑，并提供高效的渲染机制。在实际应用中，需要综合考虑应用的特点、性能要求和开发团队的技术水平，权衡使用虚拟 DOM 的利与弊。


# bootstrap
## 是什么？
前端框架，快速构建响应式设计。
## 设计原理或设计思路
1. 移动设备优先
2. 组件化设计
3. 简单文档
# 浏览器
## 浏览器端有哪些数据存储的方案
1. cookies
    - 跨页面数据传输
    - 用户身份验证
    - 获取方式
        1. ```document.cookie```
        2. ```request.cookies```
2. Web Storage
    - 本地数据存储
    - 离线应用程序
3. IndexedDB
    - 大规模数据存储
    - 离线应用程序
4. Session Storage
5. Cache Storage
    - 静态资源缓存
## 浏览器实现本地缓存主要包括协商缓存和强缓存两种机制

1. 强缓存（Cache-Control 和 Expires）：
强缓存是通过在响应头中设置相关字段来实现的，常见的字段有：
- Cache-Control：用于控制缓存的行为，常见的值包括：
- public：表示响应可以被任何中间缓存存储。
- private：表示响应只能被私有缓存存储，比如浏览器缓存。
- max-age：表示缓存的有效期，单位为秒。
- s-maxage：类似于 max-age，但仅适用于共享缓存，如 CDN。
- Expires：指定一个绝对过期时间，是一个 GMT 格式的字符串。
当浏览器发送请求时，会先检查强缓存，如果缓存有效（未过期），则直接从缓存中获取响应，不会发送请求到服务器。

2. 协商缓存（Last-Modified 和 ETag）：
协商缓存是通过服务器和浏览器之间的交互来判断缓存是否有效，常见的字段有：
- Last-Modified：表示响应的最后修改时间。
- ETag：表示响应的唯一标识符，通常是一个哈希值。
当浏览器发送请求时，会带上 If-Modified-Since 和 If-None-Match 字段，分别对应上一次的 Last-Modified 和 ETag 值。服务器通过比较这些值来判断缓存是否有效，如果有效，返回 304 Not Modified，浏览器继续使用缓存；如果无效，返回新的响应数据和相应的 Last-Modified 或 ETag 值。

通过合理配置强缓存和协商缓存，可以有效减少对服务器的请求，提高页面加载速度和用户体验。具体的缓存策略可以根据实际需求和应用场景进行选择和配置。
## 响应状态码
- 200 请求成功
- 304 Not Modified：表示缓存有效
- 400 Bad Request：表示请求无效
- 404 Not Found：表示请求的资源不存在
- 500 Internal Server Error：表示服务器内部错误

## 如果一个页面有很多图片，要怎么进行优化
1. 图片压缩：
    - 使用适当的图片格式：选择适合的图片格式，如JPEG、PNG等，根据图片的特点和需求选择最佳格式。
    - 压缩图片质量：使用图片处理工具或在线压缩工具，减少图片文件的质量，以减小文件大小。
2. 懒加载
    - 使用懒加载插件或编写自定义逻辑：将图片的加载延迟到用户滚动到可见区域时再进行加载。
    - 使用Intersection Observer API：该API可以监测元素是否进入视口，从而触发图片加载。
3. 响应式图片
    - 使用srcset属性和sizes属性：根据不同屏幕尺寸和像素密度提供不同大小的图片，并通过srcset和sizes属性指定图片的选择规则。
    - 使用picture元素：根据不同条件提供不同的图片源，使浏览器可以根据条件选择合适的图片。
4. 使用WebP或AVIF格式：
    - 尝试使用WebP或AVIF格式的图片，这些格式具有更高的压缩率和更好的图像质量，可以减小文件大小。
## http他的返回里有什么东西
- 状态行（Status Line）：包含了HTTP版本、状态码和状态消息。例如：HTTP/1.1 200 OK。

- 响应头（Response Headers）：包含了关于响应的一些元信息，例如服务器类型、日期时间、内容类型等。常见的响应头字段有：Content-Type、Content-Length、Cache-Control、Server等。

- 空行（Blank Line）：状态行和响应头之后会有一个空行，用于分隔响应头和响应体。

- 响应体（Response Body）：包含了具体的响应内容，可以是HTML、JSON、文本、图片等，根据Content-Type字段确定数据类型。

状态码（Status Code）是HTTP响应中的一部分，用于表示服务器对请求的处理结果。常见的状态码有：
2xx：表示请求成功处理。
3xx：表示重定向，要求客户端进一步处理。
4xx：表示客户端错误，如请求的资源不存在或权限不足。
5xx：表示服务器错误，如服务器内部错误或过载。
除了状态码外，响应还可能包含其他的信息，如响应体的内容、响应头中的其他字段等，具体的返回内容取决于服务器对请求的处理和配置。

## 什么是同源策略
同源策略（Same-Origin Policy）是一种浏览器安全策略，用于限制一个网页中的 JavaScript 脚本如何与其他源（域、协议或端口）的资源进行交互。同源策略要求网页中的脚本只能访问与其来源相同的资源，而对于不同源的资源访问则会受到限制。

同源策略的限制包括以下几个方面：

- 域名相同：协议、域名和端口号必须完全相同。
- JavaScript 访问限制：不同源的页面不能读取对方的 Cookie、LocalStorage 或 IndexedDB，并且无法获取对方的 DOM。
- AJAX 请求限制：XMLHttpRequest 或 Fetch API 发起的跨源请求会被浏览器阻止。

## 跨域
为了实现跨域访问，需要采取一些措施，如：

- JSONP（JSON with Padding）：通过动态创建 ```<script>``` 标签来加载外部脚本，并利用回调函数来获取数据。
```javascript
function handleResponse(data) {
  // 处理返回的数据
}

var script = document.createElement('script');
script.src = 'http://example.com/api?callback=handleResponse';
document.body.appendChild(script);
```
- CORS（Cross-Origin Resource Sharing）：服务器设置响应头来授权其他源的访问。
```javascript
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

@app.route('/api')
def api():
    # 处理请求
    return 'Response'
```
- 代理服务器：在同源的服务器上创建一个接口，用于转发跨域请求并将结果返回给前端。
# NodeJs
# flask
# ajax

 