# vue 源码解析

## 先讲点简单的，关于vue的源码
## 都知道 vue 是一个 mvvm 模式
## 什么是 mvvm 模式呢？ 说直白了 就是 数据驱动视图，视图更新数据。
### 也就是说，但内存数据更新的时候将页面上的数据也更新了。这样就是 mvvm 模式。
### 当界面上的数据变换的时候，内存中的数据也跟着在动。
### 这是我整理的 以后大家想看也也可以拿来看

[参考链接](https://cn.vuejs.org/v2/api/#productionTip)
``` js
// vue 上有什么属性 这应该是全部的属性了 大体看下就行了 不会细讲
// 全局配置 
config:{
  silent,
  optionmergeStategies,
  devtools,
  errorHandler,
  warnHandler,
  ignoredElements,
  keyCodes,
  performance,
  productionTip,
}
// 全局api
Vue: {
  extend,
  nextTick,
  set,
  delete,
  directive,
  filter,
  component,
  use,
  mixin,
  compile,
  observable,
  version
}
// 实例上的属性
vm: {
  $data,
  $props,
  $el,
  $options,
  $parent,
  $root,
  $children,
  $slots,
  $scopedSlots,
  $refs,
  $isServer,
  $attrs,
  $listeners
}
// 特殊属性
{
  key,
  ref,
  is,
  slot,
  slot-scope,
  scope
}
// 实例方法/事件
{
  $on,
  $once,
  $off,
  $emit
}
// 实例方法/数据
{
  $watch,
  $set,
  $delete
}
{
name: '', // 组件的名字
componentName: 'ElCheckbox', // 组件的名字
el:'', // 挂载节点的名字
template: '', // 节点
data:{}, // 组件上是属性
provide:{},// 父组件的值
inject:{}, // 子组件的的值
props:{}, // 父组件传递的数据
methods(){}, // 组件上面的方法
created(){}, // 组件创建之前
beforeCreated(){}, // 组件创建之后
updata(){}, // 组件更新之前
beforeupdata(){}, // 组件更新之后
mounted(){}, // 组件加载完成
beforemount(){}, // 组件加载过程
destroy(){}, // 组件销毁之前
beforeDestroy(){}, // 组件销毁之后
watch(){}, // 监听事件
computed(){}, // 计算属性
components(){}, // 引用子组件
directive(){} // 指令
mixins: {}, // 组件的扩充
extends:{}, // 组件的扩充
extend:{}, // 组件的扩充 extend > extends > mixins
render：{}, // 组件的注册
filters: {}, // 过滤器
activated(){}, // 页面打开是触发
deactivated(){}, // 页面关闭时触发
errorCaptured:{} // 错误事件 v 2.5 后才有
}

// 这应该是最全的指令了 就不复制了
参考地址: https://www.jianshu.com/p/c4a87e1b4ef7   

v-text
v-html
v-pre
v-cloak
v-once
v-if
v-else
v-else-if
v-show
v-for
v-bind
v-model
v-on
v-slot

```

### 需要了解几个属性，observe complice watcher dep
1. observer 观察者     主要是将数据进行监听
2. complice 解析指令   主要是解析指令
3. watcher  监听指令   只要是指令与上面的进行绑定
4. dep      订阅者     订阅数组.将所有的指令存放进去。实现一个队列

### 指令与属性是什么关系呢 最简单的例子
``` html
  <div id='el' v-text="text1"></div> // 上面是指令 
  <script>
     var vue = new Vue({ // 这里是方法
       el: 'el',
       data:{
         text1: '1',
       }
     })
  </script>
```
### 可以看到这里是最简单的例子
### v-text=`text1` 是如何关联 text1 的呢？
### new Vue 的时候做了什么操作

#### 上面所说的 observe 就是监听将这些监听起来 

1. 首想了解一下 new 的操作
2. new 时 是先处理的 的 data 的数据。
3. 也就是说生命的钩子的事情(咱先不说这个)
4. 将 data 数据引入进来
```js
   // Object.defineProperty 这个属性能监听数据的变化
   // 传进 三个值 第一个是 对象 第二个是 key 数组,第三个是回调函数
   Object.defineProperty({object.data},key,function(){
     set:function(new_val,old_val){
       console.log('set',new_val,old_val)
     };
     get: function(val){
       cosnole.log('get',val)
     };
   });
   // 修改 data 的时候就可以触发 对应的数据了
   data.text = '11' // 控制台输出 1 11 
    var a = data.text // 11
    // 这个 object.definedProperty 属性就可以做到当修改数据的时候就可以更新视图了
```

```js 
  // 接下来
  obsever // 将数据 全部监听起来(咱先不写)
```

#### complice 指令的解析
```js
  complete() // 指令的解析 主要的功能就是指令的解析 不同的指令有不同的方法
```

#### wacther 将 complice 的属性进行监听起来
```js
  wacther() // 这必学与其他的指令进行处理 必须用一个队列进行存放 最终达到 mvvm 的模式了。这应该是最简单的。还有很多东西需要处理。这里只说最简单的。
```

1. 如果想看源码 自行下载
2. 写得太多 也不好, 精髓就在这。如果没晕恭喜你。可以看下一篇了。
3. 下一篇是。组件的细节。也是挑重点讲。


# 大体流程基本弄懂
1. 这里细讲一下。
2. 其实是这样的,
3. new Vue({data:'',mothons:(){},el:'#app')

```js
// 先讲这几个吧。
// 从data开始
// vue 使用一个 obverse 的方法。
// 里面最主要的方法是 
// Object.defindprottype(data,key,{
//   get(){
//       // 获取数据的方法
//   },
//   set (){ 
//       // 设置属性方法
//   }
// })
// 这样我们就可以监听数据了
// 数据监听了 之后要做什么。
// 当然是 解析 指令了。
// 页面上 不是 有什么 v-if v-for {{}} v-on
// 之类的东西吗
// 接下来是 解析 complice 
// 怎么个解析发呢。
// 首先是,这样的, 是这样的。
// 有一个正则 \{\{\}\} 。
// 可以解析里面的数据
// 然后就是 生成对应的数据。
// 最后就是使用 vue 中的 watch 将数据放入进去,这边的指令与绑定的数据进行挂钩。
// click 是 用到 document.addlistend('click',function().bind(this),false)
// v-modal 是绑定在 value 上的数据。
// 就是这样子简单。
// vue 到此结束
```
## 接下来讲解 vue element ui 上的 tree 的写法。
```js
  // 上来一个子 不懂。
  // 很好
  // 其实现在我还没有完全弄清楚 vue 上面的代码呢。只不过现在还是弄清楚了这件事。
  // node。 我也不知道这样子好不好。但是还是这个吧。没有自己的想法。
  // 如果让我做的话，我也不知道怎么做。
  // 就像当初的轮播图。现在还是很简单的一件事情了。非常简单就自己写出了一个轮播图了。
  // 还是进步很大的
```


1. 如果所有的节点的操作都是这样的换那么我就知道改真么做了。
2. 所有的事情都完完全全的明白了。
3. 无所谓了.接下来证明这一点就行了。
4. 没有什么用不到的地方。现在是这样的情况。
5. 一切的一切都明白了吧。真的是明白了。你说呢。
6. 切老子累得要死。真的是累得要死。
