# 解读的是 2.6.10 版本的 vue 
## 代码参考 node_modules/vue/dist/vue.js


### 本次主要所讲的内容是 1.生命周期, 2.虚拟dom, 3.diff 算法, 4.指令(主要将的是 v-for), 5.render 函数, 6.watch
### vue 上代码

```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <style>*{margin:0px;padding:0px;}</style>
  </head>
  <body>
    <div id="app">{{ name }}</div>
  </body>
  <!-- 引用的是互联网的vue -->
  <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
  <script>
    var vue = new Vue({
      el: '#app',
      data: {
        name: 'lisi'
      }
    })
  </script>
  </html>
```

### 上面个的代码  vue 做了什么
1. 将 
    {
      el: '#app',
      data: {name: 'lisi'}
    } 
传了进去
2. 在 vue 源码里面 有一个构造函数
```js
  function Vue(options) {
    // 一切的开始 都源于这个函数
    // 这个函数中有一个 init 的方法 源码中什么都没有
    this._init(options);
  }
```
1. 接下来就是执行函数初始化函数 
2. initMixin(Vue) // 将Vue 函数传进出。主要是将 Vue 添加 _init 的方法
3. stateMixin(Vue); // 设置状态
4. eventsMixin(Vue); // 事件的绑定
5. lifecycleMixin(Vue); // 初始化生命周期。 vue 的生命周器就是重这里设置的。 vue 有8个周期,
beforeCreate created
```js
  // 生命钩子是怎样调用的呢
  // 会触发两个生命周期 beforeCreate
  // 代码是这样的
  new Vue({
    el:'#app',
    data: {name: 'lisi'},
    beforeCreate: function() {
      console.log('执行 beforeCreate')
    },
    created: function() {
      console.log('执行 created')
    }
  })
  // 当执行 vue 的时候会执行两个函数呢 [有关生命周期的链接]
  // vue 中有这样的一个函数 callHook 主要是出发钩子函数的。
  // 实例化代码传进 beforeCreate created 函数时。就会在对应时刻代码触发
  // 数据的监听 observe() 函数,是将 $option 传进去的 进行监听
  // 做了代理 proxy 将数据进行了代理  vm.data.name 简化为 vm.name 
  // 在 Object.defineProperty(obj,key,sharedPropertyDefinition) 将数据进行监听
  // 讲下 Object.defineProperty 的属性
  var obj = { one: 12}
  Object.defineProperty(obj,['one'],{
    get(){
      console.log('get')
    },
    set(newVal) {
      console.log('set', newVal)
    }
  })
  obj.one; // get
  obj.one = 123; // set 123
  // 就能对$option 的值进行监听了。当改变值时。 这就是 vue 的基本核心思想了。
```
1. 接下来是 render 函数了。render 函数是怎样的一个函数呢。
2. 现在还是 页面上的指令还是 不能解析。
3. div 上的 v-for 又是一个怎样的处理呢。接下来进行分析。
4. vue 会生成一个解析指令的函数。render 是解析指令的函数。
5. 在初始化的时候。对 $option 进行了赋值。给选项添加 render 函数
6. 创建函数 parse 将 template 与 option 传进去生成一颗抽象树
7. 抽象树创建完成了之后呢。又有什么事情发生呢。我不知道会有什么事情发什么。
8. 抽象树创建出来。就使用 render 函数。render 函数干了什么。
9. render 函数将 抽象树 

10. 记住最重要的一点就是。到最后才会使用  __path__ 这个函数将 所用的东西更新到 界面上。
11. 记住，在界面上添加一个元素的方法是 insertNode addChilds innerHTML 

```js
  var nodeOps = { // 这就是节点的 所有操作方法  无论这样都会调用这些方法的
    createElement: createElement$1,  // 创建节点
    createElementNS: createElementNS, // 创建节点
    createTextNode: createTextNode, // 创建 文字节点
    createComment: createComment,  /// 创建 共同 ？？ (我也不知道)
    insertBefore: insertBefore, // 插入节点
    removeChild: removeChild,  // 移除节点
    appendChild: appendChild,  // 添加节点
    parentNode: parentNode,    // 父节点
    nextSibling: nextSibling,   // 下一个节点
    tagName: tagName,  // 改变标签名
    setTextContent: setTextContent, // 设置文本内容
    setStyleScope: setStyleScope  // 设置样式
  }
```

12. 只是做了 diff 算法。
13. 


```js
   // 抽象树长什么样啊。
   `<div>123</div>`
```
