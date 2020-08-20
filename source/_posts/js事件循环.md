---
title: js事件循环
date: 2020-08-20 09:32:22
tags:
categories:
- 前端
- 其他
---

## 代码示例

```js
console.log('script 1')

setTimeout(() => {
  console.log('timeOut1')
  Promise.resolve().then(() => {
    console.log('then1')
  }).then(() => {
    console.log('then2')
    setTimeout(() => {
      console.log('timeOut2')
    })
  })
})

new Promise(resolve => {
  console.log('promise1')
  resolve()
  setTimeout(() => {
    console.log('timeOut3')
  })
}).then(() => {
  console.log('then4')
})

console.log('script 2')

// script1 promise1 script2 then4 timeOut1 then1  then2 timeOut3 timeOut2

```

代码执行过程

1、主程序执行输出 script1 promise1 script2 （promise resolve 会立即执行）
2、主程序执行时 宏任务栈为 ['timeOut1', 'timeOut3']
3、主程序执行时 微任务栈为 ['promise1']
4、执行微任务 `promise1` 时进入 `promise1` then 方法 输出 then4， 清空微任务栈 []
5、执行宏任务 `timeOut1` , 输出 timeOut1,并添加微任务 ['then1', 'then2'], 添加宏任务 ['timeOut1', 'timeOut3', 'timeOut2'],执行完 `timeOut1`, 宏任务栈变为 ['timeOut3', 'timeOut2']
6、执行微任务 `then1`, 输出 then1； 执行微任务 `then2`, 输出 then2。清空微任务 []
7、执行宏任务 `timeOut3`, 输出 timeOut3。宏任务栈变为['timeOut2']
8、执行宏任务 `timeOut2`, 输出 timeOut2。清空宏任务栈 []。

执行完此段代码。


## 参考
+ [详解JavaScript中的Event Loop（事件循环）机制](https://zhuanlan.zhihu.com/p/33058983)
+ [浏览器中的事件循环机制](https://zhuanlan.zhihu.com/p/30034547)