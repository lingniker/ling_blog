Vue 是一个构造函数。
当 new 的时候会生成一个实例。

1. 将参数传进去，对入参进行处理。

```js
  new Vue({});
```

2. 首先，vue 会触发 钩子函数，调用的是 beforeCreate. 会执行里面的函数，什么都没有处理.
```js
  new Vue({
    beforeCreate(){
      // 事先处理里面的函数。
    }
  })
```

3. 然后， vue 就会初始数据， 将入参 data 进行处理， 
对 data 上的数据进行观察。放到一个订阅队列上。observe 

```js
  new Vue({
    data(){
      return {
        // 将函数观察起来。 主要是 defineProtype
      }
    }
  })
```

4. 完成 再调用 create 钩子函数。 这样 vm 的实例上，就会出现 data 上的数据。
```js
    new Vue({
      create(){
        // 这个时候, 调用里面的数据
      }
  })
```

5. 
compile 解决的div生成了一颗抽象树ast.
将抽象树转换为vdom.
vdom 是 render 生成的。主要由 vm._renderProxy, vm.$createElement 这两个变量生成的。
抽象树这样就能完成了最后的更新了。抽象树与render函数还是这样子的不一样啊。
不仅仅是这样的事情还有其它的事情要发生。仅仅是这样的事情。不仅仅是这样的事情吧。
这样理解没有错吧。你是理解是什么呢。你也不知道我理解的对不对吧。还有什么呢。不知道就对了。
想象之中我是很强的。非常的强大的。你能理解吗。不知道吧。我也不知道啊。你想干嘛啊。
我要去写一个框架。非常棒的一个框架。你们都不知道的框架。

调用的是钩子函树。render 函数已经准备好。




5. 然后就是 reader, 会解析 templater 下的节点。 就是解析节点。
解析节点上的 v-if,v-show,v-for, template 下的对应的数据。
解析完之后，会生成一颗抽象数。 抽象数。抽象树会生成真实的节点。
并没有挂载到浏览器的节点上。

```html
  <template>
    <div v-if="ifFlag">
      <div v-show="showFlag"></div>
      <div v-for="item in data">{{ item.name }}</div>
    </div>
  </template>
```

```js
   // 会生成抽象树 等所有指令
   _c('div',{ifFlag: "true"},[
     {
       _c:_c("div",{showFlag: ""},[]),
     },{
       _l:_l("div",{data:"d"},[
         {
           _c("ns",{},["item.data"])
         }]
       )
     }
   ])
```

```js
  // 抽象树 ast, 最后会生成节点
  // ast 都会变成 
  <div>
    <div></div>
    <div>
      ling
    </div>
    <div>
      yuan
    </div>
  </div>
```
// 现在，还没不考虑diff算法(以后讨论)

6. 然后就是 beforeMount， 但是没有挂载到 页面的 dom.




7.   dom 进行 watch 数据监听，与 data 数据进行联动，
最后就是 mounted 将 dom 挂到真实的页面上。

```html
<template>
  <div>
    <div v-if="ifFlag"></div>
  </div>
</template>
<script>
  export default {
    data(){
      return ifFlag;
    }
  }
</script>
```
调用的是内部的 Watch 的进行对 这样的监听

// 产生联动效果。



8. 然后调用，生命周期的 mounted 函数。

---------------------------(前面就是整个vue的过程)------------------------------------

1.  下面再说下可能 一些技术算法
你们常说的 diff 算法。(本质是hash,将数据结构打散，就是空间复杂复杂度,与时间复杂度。将o(n3)变成o(n));

还有就是抽象树， 这个你们应该很少了解吧。
抽象树 ast 一般会再编译出现。v8 在对 js 的解析过程中，也会变成抽象树ast.还没认真去系统的了解v8,所以就
不深入讨论。

抽象树是什么呢。

```js
  // 这就是抽象树
  _c("div",
      {data:{name: "btn"}},[
         {
          _c("div",{data:{name: "btn"}},[])
        }
     ])
```


// 留给大家一个问题 为什么 谷歌 是 0(n3)。 而 react, vue的diff 算法是 o(n)
// 难道谷歌公司，没有考虑到，下期再见

觉得好就随意转载。出产人： lingyuanping。