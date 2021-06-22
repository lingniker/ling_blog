# promise 介绍
### async await 阻塞了进程
### 目前为止 阻塞进出的有 alart()。 ajax 的同步请求。 还有一个就是 async await 只能在函数中使用。
### js 永远是单线程。也就是说，在同一时刻只能执行一条语句。

### 比如说两个算法 

```js
  var a = 1;
  var b = 2;
  function fn(a,b, key){
    // 代码的执行是从上往下走。
    for(var i = 0; i < 100 ; i ++){
      console.log("key ", key ," i ", i , a + b);
    }
  }
  setTimeout(()=>{
    fn(3,4, 'a');
  },0)
  setTimeout(()=>{
    fn(2,3,'b');
  },0)
  setTimeout(()=>{
    fn(1,2,'c');
  },0)
  fn(1,2,'d')
```

### setTimeout nextTick 
1. 先执行 nextTick ,再执行 setTimeout
2. 只有nodejs 上有 process.nextTick()
```js
  setTimeout(function () {
    console.log(123) // 最后执行
  },0)
```



### 其实我认为回调一直很好用


### promise 的出现是为了什么

### promise 没有出现之前的事情是这样的。

```js
    // 请求数据
    function updataState(){
      // 更新页面状态
    };
    ajax('url',function(res){
      // 因为是 异步的 所以只能在回调函数做数据处理。
      ajax("url",function(res){
        // 再次请求数据
        ajax('url', function(res){
          // 第三次请求数据了。
          ajax("url", function(res){
            // 已经嵌套很多层了
            updataState() // 嵌套这么多层 终于 能跟新状态了。 看起来好像也没有什么难度。
          })
        })
      })
    })

    var ajax = new Promise(function(rel,rej){
      ajax('url',function(res){
        rel(res);
      })
    })
    // 就没有什么层级的关系, 一直 then 下去就行了。
    ajax.then(function(res){
      return newRes;
    }).then(function(res){
      return newRes;
    }).then(function(res){
      return res;
    })

    console.log("会再then的函数前调用, 因为 then 内部是使用 setTimeout, 是异步");

    // 你控制的好的话，第一种方法也是可以使用的。promise 就是解决回调多长的问题
```

#### 解析来就看看 promise 的源码. 代码量不多.

#### 首先上面介绍了 promise 简单的使用方法。

#### promise 是要让痛苦的回调，变得链式调用

```js
  // 接下来呢

  function Promise (rel,rej) { // 输入的是两个函数
    // 这样就行 rel , rej 的什么情况下会真正的使用呢，这个我也不知道啊。
    rel("ha ha");
    rej("ha ha");
  }
  Promise.propotype.then = (cb) => {// 其实是将你cb 函数放在一个队列中。
    Promise.arr.push(cb);
  }
  Promise.propotype.arr = []
  Promise.propotype.done = function () {
    for(var i = 0; i < Promise.propotype.arr.length; i++){
      setTimeout(function(){ // 最后使用的还是setTimeout // 异步
        Promise.propotype.arr(res);
      },0)
    }
  }
```

#### 这就是一个简单的 Promise 函数。

#### 改一下函数名都是一样的  myPromise。完完全全可以替换 Promise api 。 ie 没有 promise。

#### 能理解一下 异步。

#### 出一个经常考的题 如果你们能理解就很好了

```js
  console.log("a");
  var promiseFn = new Promise (function(rel,rej){
    console.log("b");
    setTimeout(()=>{
      console.log("c")
    },0)
  });
  setTimeout(()=>{
    console.log("d");
  });

  promiseFn.then(()=>{
    console.log("e");
  });

  console.log("f");

```

### 下一节 async/awite 不一样的理解。你们的认知好像又被改变了。