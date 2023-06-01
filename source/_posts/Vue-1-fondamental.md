---
title: Vue-1-前置知识点
date: 2023-05-29 16:39:45
tags:
    - vue
    - JavaScript
    - node
categories: 
    - JavaScript
cover: /images/vue_cover.png
---
# node.js
这里的目的仅在于了解node.js的模块化
## 1. 内置模块
### 1. fs 文件系统模块
如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：
```javascript
const fs = require('fs')
```
#### 1.1 什么是 fs 文件系统模块
fs 模块是 Node.js 官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求
#### 1.2 读取指定文件中的内容
使用 fs.readFile() 方法，可以读取指定文件中的内容，语法格式如下：
```javascript
fs.readFile(path[, options], callback)

const fs = require('fs')

fs.readFile('./files/11.txt', 'utf8', function(err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})
```
参数解读：
- 参数1：必选参数，字符串，表示文件的路径。
- 参数2：可选参数，表示以什么编码格式来读取文件。
- 参数3：必选参数，文件读取完成后，通过回调函数拿到读取的结果。

#### 1.3 向指定的文件中写入内容
使用 fs.writeFile() 方法，可以向指定的文件中写入内容，语法格式如下：
```javascript
fs.writeFile(file, data[, options], callback)

// 1. 导入 fs 文件系统模块
const fs = require('fs')

// 2. 调用 fs.writeFile() 方法，写入文件的内容
//    参数1：表示文件的存放路径
//    参数2：表示要写入的内容
//    参数3：回调函数
fs.writeFile('./files/3.txt', 'ok123', function(err) {
  // 2.1 如果文件写入成功，则 err 的值等于 null
  // 2.2 如果文件写入失败，则 err 的值等于一个 错误对象
  // console.log(err)

  if (err) {
    return console.log('文件写入失败！' + err.message)
  }
  console.log('文件写入成功！')
})
```
参数解读：
- 参数1：必选参数，需要指定一个**文件路径的字符串**，表示文件的存放路径。
- 参数2：必选参数，表示要写入的内容。
- 参数3：可选参数，表示以什么格式写入文件内容，**默认值是 utf8**。
- 参数4：必选参数，文件写入完成后的回调函数
  
#### 1.4 fs 模块 - 路径动态拼接的问题

在使用 fs 模块操作文件时，如果提供的操作路径是以 ./ 或 ../ 开头的相对路径时，很容易出现路径动态拼接错误的问题。

原因：代码在运行的时候，会以执行 node 命令时所处的目录，动态拼接出被操作文件的完整路径。

解决方案：在使用 fs 模块操作文件时，直接提供完整的路径，不要提供 ./ 或 ../ 开头的相对路径，从而防止路径动态拼接的问题
```javascript
const fs = require('fs')

// 出现路径拼接错误的问题，是因为提供了 ./ 或 ../ 开头的相对路径
// 如果要解决这个问题，可以直接提供一个完整的文件存放路径就行
fs.readFile('./files/1.txt', 'utf8', function(err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})

// 移植性非常差、不利于维护
fs.readFile('C:\\Users\\escook\\Desktop\\Node.js基础\\day1\\code\\files\\1.txt', 'utf8', function(err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})

// __dirname 表示当前文件所处的目录
// console.log(__dirname)

fs.readFile(__dirname + '/files/1.txt', 'utf8', function(err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})
```

### 2. path 路径模块
#### 2.1 什么是 path 路径模块
path 模块是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求。
#### 2.2 路径拼接
**注意：今后凡是涉及到路径拼接的操作，都要使用 path.join() 方法进行处理。不要直接使用 + 进行字符串的拼接。**
```javascript
const path = require('path')
const fs = require('fs')

// 注意：  ../ 会抵消前面的路径
// const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
// console.log(pathStr)  // \a\b\d\e

// fs.readFile(__dirname + '/files/1.txt')

fs.readFile(path.join(__dirname, './files/1.txt'), 'utf8', function(err, dataStr) {
  if (err) {
    return console.log(err.message)
  }
  console.log(dataStr)
})
```

#### 2.3 获取路径中的文件名

```javascript
const path = require('path')

// 定义文件的存放路径
const fpath = '/a/b/c/index.html'

// const fullName = path.basename(fpath)
// console.log(fullName)

const nameWithoutExt = path.basename(fpath, '.html')
console.log(nameWithoutExt)
```
#### 2.4 获取路径中的文件扩展名

```javascript
const path = require('path')

// 这是文件的存放路径
const fpath = '/a/b/c/index.html'

const fext = path.extname(fpath)
console.log(fext)
```
### http 模块
#### 3.1 什么是 http 模块
http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。
#### 3.2 进一步理解 http 模块的作用
在 Node.js 中，我们不需要使用 IIS、Apache 等这些第三方 web 服务器软件。因为我们可以基于 Node.js 提供的http 模块，通过几行简单的代码，就能轻松的手写一个服务器软件，从而对外提供 web 服务。
#### 3.3 创建最基本的 web 服务器
最基本的
```javascript
// 1. 导入 http 模块
const http = require('http')
// 2. 创建 web 服务器实例
const server = http.createServer()
// 3. 为服务器实例绑定 request 事件，监听客户端的请求
server.on('request', function (req, res) {
  console.log('Someone visit our web server.')
})
// 4. 启动服务器
server.listen(8080, function () {  
  console.log('server running at http://127.0.0.1:8080')
})
```
req请求对象
只要服务器接收到了客户端的请求，就会调用通过 server.on() 为服务器绑定的 request 事件处理函数。
如果想在事件处理函数中，访问与客户端相关的数据或属性，可以使用如下的方式：

```javascript
const http = require('http')
const server = http.createServer()
// req 是请求对象，包含了与客户端相关的数据和属性
server.on('request', (req, res) => {
  // req.url 是客户端请求的 URL 地址
  const url = req.url
  // req.method 是客户端请求的 method 类型
  const method = req.method
  const str = `Your request url is ${url}, and request method is ${method}`
  console.log(str)
  // 调用 res.end() 方法，向客户端响应一些内容
  res.end(str)
})
server.listen(80, () => {
  console.log('server running at http://127.0.0.1')
})

```
解决中文乱码问题

```javascript
const http = require('http')
const server = http.createServer()

server.on('request', (req, res) => {
  // 定义一个字符串，包含中文的内容
  const str = `您请求的 URL 地址是 ${req.url}，请求的 method 类型为 ${req.method}`
  // 调用 res.setHeader() 方法，设置 Content-Type 响应头，解决中文乱码的问题
  res.setHeader('Content-Type', 'text/html; charset=utf-8')
  // res.end() 将内容响应给客户端
  res.end(str)
})

server.listen(80, () => {
  console.log('server running at http://127.0.0.1')
})

```
#### 3.4 根据不同的 url 响应不同的 html 内容
核心实现步骤
① 获取请求的 url 地址
② 设置默认的响应内容为 404 Not found
③ 判断用户请求的是否为 / 或 /index.html 首页
④ 判断用户请求的是否为 /about.html 关于页面
⑤ 设置 Content-Type 响应头，防止中文乱码
⑥ 使用 res.end() 把内容响应给客户端


动态响应内容


```javascript
const http = require('http')
const server = http.createServer()

server.on('request', (req, res) => {
  // 1. 获取请求的 url 地址
  const url = req.url
  // 2. 设置默认的响应内容为 404 Not found
  let content = '<h1>404 Not found!</h1>'
  // 3. 判断用户请求的是否为 / 或 /index.html 首页
  // 4. 判断用户请求的是否为 /about.html 关于页面
  if (url === '/' || url === '/index.html') {
    content = '<h1>首页</h1>'
  } else if (url === '/about.html') {
    content = '<h1>关于页面</h1>'
  }
  // 5. 设置 Content-Type 响应头，防止中文乱码
  res.setHeader('Content-Type', 'text/html; charset=utf-8')
  // 6. 使用 res.end() 把内容响应给客户端
  res.end(content)
})

server.listen(80, () => {
  console.log('server running at http://127.0.0.1')
})
```
## 模块化
### 1. 模块化的基本概念
#### 1.1 什么是模块化
**模块化**是指解决一个**复杂问题**时，自顶向下逐层把**系统划分成若干模块的过程**。对于整个系统来说，模块是**可组合、分解和更换的单元**.

编程领域中的模块化，就是**遵守固定的规则**，把一个**大文件**拆成**独立并互相依赖**的**多个小模块**。

把代码进行模块化拆分的好处：
- ① 提高了代码的**复用性**
- ② 提高了代码的**可维护性**
- ③ 可以实现**按需加载**

#### 1.2 模块化规范
**模块化规范**就是对代码进行模块化的拆分与组合时，需要遵守的那些规则。

### 2. Node.js 中的模块化
#### 2.1 Node.js 中模块的分类

Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：
- 内置模块（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）
- 自定义模块（用户创建的每个 .js 文件，都是自定义模块）
- 第三方模块（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载

#### 2.2 加载模块
使用强大的 require() 方法，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用。例如：
```javascript
const fs = require('fs')
const custom = require('./custom.js')
const moment = require('moment')
```
#### 2.3 Node.js 中的模块作用域
##### 1. 什么是模块作用域
和函数作用域类似，在自定义模块中定义的变量、方法等成员，**只能在当前模块内被访问**，这种模块级别的访问限制，叫做模块作用域。

##### 2. 模块作用域的好处
防止了全局变量污染的问题

#### 2.4 向外共享模块作用域中的成员
##### 1. module 对象
在每个 .js 自定义模块中都有一个 module 对象，它里面存储了和当前模块有关的信息，打印如下：

- id：当前模块的标识符，通常是模块文件的绝对路径。
- exports：当前模块使用 module.exports 导出的对象。
- parent：当前模块的父级模块。
- filename：当前模块的文件名。
- loaded：返回一个布尔值，指明该模块是否已经加载完毕。
- children：返回一个子模块数组，表示该模块所引入的所有子模块。
- paths：返回一个字符串数组，表示 Node.js 模块解析器所使用的路径列表。

##### 2. module.exports 对象
在自定义模块中，可以使用 module.exports 对象，将模块内的成员共享出去，供外界使用。
外界用 require() 方法导入自定义模块时，得到的就是 module.exports 所指向的对象。

##### 3. 共享成员时的注意点
使用 require() 方法导入模块时，导入的结果，**永远以 module.exports 指向的对象为准.**

##### 4. exports 对象
由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 exports 对象。**默认情况下，exports 和 module.exports 指向同一个对象**。最终共享的结果，还是**以 module.exports 指向的对象为准**。

##### 5. exports 和 module.exports 的使用误区
时刻谨记，require() 模块时，得到的永远是 module.exports 指向的对象

**为了防止混乱，建议大家不要在同一个模块中同时使用 exports 和 module.exports**

#### 2.5 Node.js 中的模块化规范
Node.js 遵循了 CommonJS 模块化规范，CommonJS 规定了模块的特性和各模块之间如何相互依赖。
CommonJS 规定：
1. 每个模块内部，module 变量代表当前模块。
2. module 变量是一个对象，它的 exports 属性（即 module.exports）是对外的接口。
3. 加载某个模块，其实是加载该模块的 module.exports 属性。require() 方法用于加载模块

  
### 4. 模块的加载机制
#### 4.1 优先从缓存中加载
**模块在第一次加载后会被缓存**。 这也意味着多次调用 **require()** 不会导致模块的代码被执行多次。

**注意：不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。**

#### 4.2 内置模块的加载机制
内置模块是由 Node.js 官方提供的模块，**内置模块的加载优先级最高**。
例如，require('fs') 始终返回内置的 fs 模块，即使在 node_modules 目录下有名字相同的包也叫做 fs
#### 4.3 自定义模块的加载机制

使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 ../ 
这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。


同时，在使用 require() 导入自定义模块时，如果省略了文件的扩展名，则 Node.js 会按顺序分别尝试加载以下的文件：
1. 按照确切的文件名进行加载
2. 补全 .js 扩展名进行加载
3. 补全 .json 扩展名进行加载
4. 补全 .node 扩展名进行加载
5. 加载失败，终端报错

#### 4.4 第三方模块的加载机制

如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父
目录开始，尝试从 /node_modules 文件夹中加载第三方模块。
如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。
例如，假设在 'C:\Users\itheima\project\foo.js' 文件里调用了 require('tools')，则 Node.js 会按以下顺序查找：
1. C:\Users\itheima\project\node_modules\tools
2. C:\Users\itheima\node_modules\tools
3. C:\Users\node_modules\tools
4. C:\node_modules\tools

#### 4.5 目录作为模块
当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：
1. 在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require() 加载的入口
2. 如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
3. 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'

# ES6模块化与异步编程高级用法
## ES6 模块化
## Promise
### 1. 回调地狱
多层回调函数的相互嵌套，就形成了回调地狱。

- 代码耦合性太强，牵一发而动全身，难以维护
- 大量冗余的代码相互嵌套，代码的可读性变差
#### 1.1 如何解决回调地狱的问题
为了解决回调地狱的问题，ES6（ECMAScript 2015）中新增了 Promise 的概念
#### 1.2 Promise 的基本概念
① **Promise 是一个构造函数**
- 我们可以创建 Promise 的实例 const p = new Promise()
- new 出来的 Promise 实例对象，代表一个异步操作
② **Promise.prototype 上包含一个 .then() 方法**
- 每一次 new Promise() 构造函数得到的实例对象，
- 都可以通过**原型链**的方式访问到 .then() 方法，例如 **p.then()**
③ **.then() 方法用来预先指定成功和失败的回调函数**
- p.then(成功的回调函数，失败的回调函数)
- p.then(**result => { }, error => { }**)
- 调用 .then() 方法时，成功的回调函数是必选的、失败的回调函数是可选的
  
### 2. 基于回调函数按顺序读取文件内容
```javascript
fs.readFile('path', 'utf8', (err1, r1) => {
  fs.readFile('path', 'utf8', (err2, r2) => {
    fs.readFile('path', 'utf8', (err2, r2) => {
    })
  })
})
```
### 3. 基于 then-fs 读取文件内容

调用 then-fs 提供的 readFile() 方法，可以异步地读取文件的内容，它的返回值是 Promise 的实例对象。因
此可以调用 .then() 方法为每个 Promise 异步操作指定成功和失败之后的回调函数。示例代码如下：

```javascript
import thenFs from 'then-fs'

thenFs.readFile('./files/1.txt', 'utf8').then((r1) => {console.log(r1)})
thenFs.readFile('./files/2.txt', 'utf8').then((r2) => {console.log(r2)})
thenFs.readFile('./files/3.txt', 'utf8').then((r3) => {console.log(r3)})
```
### 4. 基于 Promise 按顺序读取文件的内容
```javascript
import thenFs from 'then-fs'

thenFs
.readFile('./files/11.txt', 'utf8')
  .catch((err) => {
    console.log(err.message)
  })
  .then((r1) => {
    console.log(r1)
    return thenFs.readFile('./files/2.txt', 'utf8')
  })
  .then((r2) => {
    console.log(r2)
    return thenFs.readFile('./files/3.txt', 'utf8')
  })
  .then((r3) => {
    console.log(r3)
  })
```
### 5. Promise.all() 方法
在 `Promise.all()` 方法中，如果其中有一个 `Promise` 对象的状态变为 `rejected`，则整个方法立即停止执行，并且返回一个 `rejected` 状态的 `Promise` 对象，即使其它 `Promise `对象的状态是 `fulfilled`，也不会再执行 `.then()` 方法，直接进入 `.catch()` 方法中处理异常情况。
因此，在使用 `Promise.all()` 方法时，需要确保其中的所有 `Promise` 对象都能够正常执行，以避免出现异常情况。

```javascript
import thenFs from 'then-fs'

const promiseArr = [
  thenFs.readFile('./files/3.txt', 'utf8'),
  thenFs.readFile('./files/2.txt', 'utf8'),
  thenFs.readFile('./files/1.txt', 'utf8'),
]

Promise.race(promiseArr).then(result => {
  console.log(result)
})
```
### 6 Promise.race() 方法
Promise.race() 方法会发起并行的 Promise 异步操作，只要任何一个异步操作完成，就立即执行下一步的
.then 操作（赛跑机制）。示例代码如下:
```javascript
import thenFs from 'then-fs'

const promiseArr = [
  thenFs.readFile('./files/3.txt', 'utf8'),
  thenFs.readFile('./files/2.txt', 'utf8'),
  thenFs.readFile('./files/1.txt', 'utf8'),
]

Promise.race(promiseArr).race(result => {
  console.log(result)
})
```
`Promise.all()` 和 `Promise.race()` 内部的异步操作是同时进行的，而不是顺序执行的。它们通过 JavaScript 的异步机制进行并行处理，即将异步操作放到 event loop 中，当异步操作完成时，通过回调函数返回结果。

在 JavaScript 中，异步操作依赖于浏览器或者 `Node.js` 运行环境的支持。这些运行环境内部都维护了一个线程池用于执行异步任务，也就是说，JavaScript 本身并不会启动新的线程来执行异步任务。当我们需要执行大量异步任务时，线程池中的线程数量可能会变得不足以处理所有的任务，这时就有可能会出现线程饱和的情况，即某些异步任务需要等待其它任务执行完成后才能开始执行。这样就会导致异步操作的顺序执行，而非同时进行。

具体而言，如果你有一万个异步操作需要执行，由于每个异步操作会占用一定的系统资源，因此无论是在浏览器环境还是在 `Node.js` 环境下，都需要考虑到系统的性能和资源消耗问题。如果同时启动一万个异步操作，系统可能会因为资源不足而崩溃或者变得极其缓慢，因此需要对任务进行适当分批执行，以避免系统资源瓶颈的产生。可以使用一些工具和库来实现任务分批执行，例如 `Node.js` 中的 `async` 和 `bluebird` 库。

### 7 async/await 的基本使用
```javascript
import thenFs from 'then-fs'

console.log('A')
async function getAllFile() {
  console.log('B')
  const r1 = await thenFs.readFile('./files/1.txt', 'utf8')
  console.log(r1)
  const r2 = await thenFs.readFile('./files/2.txt', 'utf8')
  console.log(r2)
  const r3 = await thenFs.readFile('./files/3.txt', 'utf8')
  console.log(r3)
  console.log('D')
}

getAllFile()
console.log('C')

async function getAllFile() {
  console.log('B')
  const r1 =  thenFs.readFile('./files/1.txt', 'utf8')
  console.log(r1)
  const r2 =  thenFs.readFile('./files/2.txt', 'utf8')
  console.log(r2)
  const r3 =  thenFs.readFile('./files/3.txt', 'utf8')
  console.log(r3)
  console.log('D')
}
```
代码会按照以下顺序输出内容：

```
A
B
C
(1.txt 的内容)
(2.txt 的内容)
(3.txt 的内容)
D
```

首先同步执行的部分会输出 `A` 和 `C`，然后调用 `getAllFile()` 函数。在函数内部，第一个 `await` 方法会执行文件读取操作，因为文件操作是异步的，所以代码会继续执行第二个和第三个 `await` 方法。当第一个文件读取操作执行完成之后，会输出第一个文件的内容，然后依次输出第二个和第三个文件的内容，最后输出 `D`。在整个过程中，函数内部的代码是按照顺序执行的，没有其他异步操作会干扰它的执行顺序。

## 宏任务和微任务
### 1. 什么是宏任务和微任务
JavaScript 把异步任务又做了进一步的划分，异步任务又分为两类，分别是：
① 宏任务（macrotask）
- 异步 Ajax 请求、
- setTimeout、setInterval、
- 文件操作
- 其它宏任务
② 微任务（microtask）
- Promise.then、.catch 和 .finally
- process.nextTick
- 其它微任务
  
每一个宏任务执行完之后，都会检查是否存在待执行的微任务，
如果有，则执行完所有微任务之后，再继续执行下一个宏任务。

## 打包
webpack
```javascript
const path = require('path')

// 1. 导入插件，得到构造函数
const HtmlPlugin = require('html-webpack-plugin')
// 2. 创建插件的实例对象
const htmlPlugin = new HtmlPlugin({
  template: './src/index.html',
  filename: './index.html',
})

const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const cleanPlugin = new CleanWebpackPlugin()

module.exports = {
  mode: 'development', // development  production
  // eval-source-map 仅限在开发模式下使用
  // devtool: 'eval-source-map',
  // 生产环境下，建议关闭 SourceMap 或将 devtool 的值设置为 nosources-source-map
  // devtool: 'nosources-source-map',
  // devtool: 'source-map',
  // 指定打包的入口
  entry: path.join(__dirname, './src/index.js'),
  // 指定打包的出口
  output: {
    // 表示输出文件的存放路径
    path: path.join(__dirname, './dist'),
    // 表示输出文件的名称
    filename: 'js/bundle.js',
  },
  plugins: [htmlPlugin, cleanPlugin], // 3. 挂载插件的实例对象
  devServer: {
    open: true,
    host: '127.0.0.1',
    port: 80,
  },
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      // { test: /\.jpg|png|gif$/, use: 'url-loader?limit=22228' }
      {
        test: /\.jpg|png|gif$/,
        use: {
          loader: 'url-loader',
          options: {
            limit: 22228,
            outputPath: 'image',
          },
        },
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            plugins: ['@babel/plugin-proposal-class-properties'],
          },
        },
      },
    ],
  },
}
```