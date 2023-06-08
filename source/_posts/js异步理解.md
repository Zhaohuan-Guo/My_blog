---
title: js异步理解
date: 2023-06-06 22:16:41
tags: 
    - JavaScript
categories: JavaScript
cover: https://eachscape.com/wp-content/uploads/2016/07/JScript.png
---
# callback hell
回调函数是 JavaScript 中最基础的处理异步操作的方式。它通过将一个函数作为参数传递给另一个函数，在异步操作完成后调用回调函数来处理结果。回调函数通常采用错误优先的约定，即第一个参数为错误对象（如果有错误发生），后续参数为操作结果。

阅读起来及其复杂
```js
function getDate(data, callback) {
    setTimeout(() => {
        console.log('get the data')
        callback(data)
    }, 1000)
}

function processData(data, callback) {
    setTimeout(() => {
        console.log('resolve the data:', data)
        callback(data.toUpperCase())
    }, 1000)
}

function displayData(data, callback) {
    setTimeout(() => {
        console.log('display the data:', data)
        callback()
    }, 1000)
}

function displayData(data, callback) {
    console.log('work')
    callback()
}



getDate('the raw data', (data) => {
    processData(data, (processData) => {
        displayData(processData, () => {
            console.log('operations completed')
        })
    })
})

// ouput:
// get the data
// resolve the data: the raw data
// display the data: THE RAW DATA
// operations completed
```

# promise
Promise 是一种更为现代的异步处理方式，它通过链式调用的方式提供了更清晰和可读性更好的代码结构。Promise 提供了 `resolve` 和 `reject` 两个函数来表示异步操作的完成状态，通过调用这些函数来传递结果或错误。

```js
function getDate(data) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('get the data')
            resolve(data)
        }, 1000)
    })
}

function processData(data) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('resolve the data:', data)
            resolve(data.toUpperCase())
        }, 1000)
    })   
}

function displayData(data) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log('display the data:', data)
            resolve()
        }, 1000)
    })
}


getDate('the raw date')
.then(data => processData(data))
.then(processData => displayData(processData))
.then(() => console.log('operation completed'))
```

# async

Async/Await 是基于 Promise 的一种语法糖，它使异步代码的编写更类似于同步代码，提供了更好的可读性和简洁性。通过在函数前加上 `async` 关键字，可以定义一个异步函数，在其中使用 `await` 关键字等待 Promise 对象的完成，并以同步的方式获取结果。

```js
function getDate(data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (data === 'error') {
        reject('An error occurred while getting the data')
      } else {
        console.log('get the data')
        resolve(data)
      }
    }, 1000)
  })
}

function processData(data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (!data) {
        reject('Error in processing data: Invalid data')
      } else {
        console.log('resolve the data:', data)
        resolve(data.toUpperCase())
      }
    }, 1000)
  })
}

function displayData(data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (!data) {
        reject('Error in displaying data: No data to display')
      } else {
        console.log('display the data:', data)
        resolve()
      }
    }, 1000)
  })
}

async function main(data) {
  try {
    const rawData = await getDate(data)
    const processedData = await processData(rawData)
    await displayData(processedData)
    console.log('operation done')
  } catch (error) {
    console.error('An error occurred:', error)
  }
}

main('the raw data')
```

# Generator 协程

Generator 是一种特殊的函数，可以在执行过程中暂停和恢复控制流。通过使用 yield 关键字，在 Generator 函数内部可以生成一个可迭代的对象，通过调用迭代器的 next() 方法逐步执行 Generator 函数的代码。Generator 函数可以与回调函数或 Promise 结合使用。

**用来理解如何将异步操作写成同步写法**
**多次调用效果类似promise.all，谁先执行好就输出谁**

```js
async function main(data) {
    try {
        const rawData = await getDate(data)
        const processedData = await processData(rawData)
        await displayData(processedData)
        console.log('operation done')
    } catch (error) {
        console.error('An error occurred:', error)
    }
}

function* mainGenerator(data) {
    try {
        const rawData = yield getDate(data)
        const processedData = yield processData(rawData)
        yield displayData(processedData)
        console.log('operation done')
    } catch (error) {
        console.error('An error occurred:', error)
    }
}


function runCoroutine(generator, data) {
    const iterator = generator(data)
    function step(value) { 
        const result = iterator.next(value)
        if (!result.done) {
            if (result.value instanceof Promise) {
                return result.value.then(step)
            } else {
                return step(result.value)
            }
        } else {
            return Promise.resolve(result.value)
        }
    }
    return step()
}

main('data 123456')
runCoroutine(mainGenerator, 'data 987654')

// result:
// get the data
// get the data
// resolve the data: data 123456
// resolve the data: data 987654
// display the data: DATA 123456
// operation done
// display the data: DATA 987654
// operation done
```