# promise 介绍
## async await 阻塞了进程
### 目前为止 阻塞进出的有 alart()。 ajax 的同步请求。 还有一个就是 async await 只能在函数中使用。
### js 永远是单线程。

### setTimeout nextTick 
1. 先执行 nextTick ,再执行 setTimeout
2. 只有nodejs 上有 process.nextTick()
```js
  setTimeout(function () {
    console.log(123) // 最后执行
  },0)
```