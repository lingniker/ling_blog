# 这是系统的对vue的解析
## 会一点一点的讲解
## 其实将api文档弄懂就行了,你就是一个合格的vue开发者了。而且很简单。非常的简单。
## 如果是刚接触的话(或者是初学者的话，最好看api文档)，看懂就会了。我就简单的讲下就行。
## [api文档]https://cn.vuejs.org/v2/api/

```html
<! document type>
<html>
<hread>
</hread>
<body>
  <div id="app"></div>
  <script src="vue.js"></script>
  <script>
  // 简单的说下吧,数据是这样的，没有什么的,
  // vue 有属性。
  // 全局方法。
  // 指令。
  // 没有了, 就这三个。简单吧。够简单吧。

  // 那么属性是什么
  // var vm = new Vue({ // 说明 vue 是一个构造函数。
  //   data: {}, // 通常我们定义的数据就在这里面
  // })
  // vm.extend() // 这就是全局方法
  // 指令就是 <div v-if="show"></div> v-if 就是指令
  // 没了完了。vue就是这样使用。超级简单。看下 api 就懂了。

  // 然后呢我们就是对上面的几个函数，并根据 vue 源码进行分析。我使用的vue是 es5 的语法。所以接下来，我都会以 es5。进行分析。
  // js 代码是从上到下去解析的。
  // 现在呢。我们看看我们的代码是这么写的。
  // 写出一个完整的例子吧。
  // <div id="app" v-if="show">{{ text }}</div>
  // <script>
  // var vm = new Vue({
  //   el: 'app'
  //   data: {
  //     show: true,
  //     text: '内容'
  //   }
  // })
  // <\script>
  // 然后页面上就有内容了。是不是很简单。 现在我们来分析下源码。
  // 在没有使用 vue 的时候。如果使用jquery 的同学应该知道。我们改变页面是通过 
  // $('#app').html('内容') 或者是原生的 document.getElementById('app').innerHTML = "内容" 
  // 这种方法去改变 节点的内容。 
  // 还有 $('#app').html('内容') 其实是转换成了==> document.getElementById('app').innerHTML = "内容"
  // vue 最终也是调用 节点上的 el.innerHTML = '内容' 去改变浏览器的样式的。
  // 万变不离其中。
  // 但是vue 有个好处。就是改变属性，就能改变节点了。
  // 这是怎样的操作呢。
  // var vm = new Vue({
  //   el: '#app',
  //   data: {
  //     show: true,
  //     text: '内容'  
  //   },
  //   mounted: function() { // 这是生命周期的挂载完
  //      this.text = '改变内容'
  //   }
  // })
  // 页面改变内容了。
  // 这就是你们所说的 mvvm 模式。改变数据然后可以改变视图。改变视图就能反过来改变界面。
  // 这个视频还要改动。但是大体还是不会改很多东西。各位老大啊。给点意见吧。
  // 现在我们开始分析代码吧。

  // 从 vue 的源码中。
  // 我们看到了一个函数, 一个构造函数。(在源码的 5067 行)。你们先别看源码。听我说。Vue 就是一个构造函数。
  // function Vue(option) { 一切从这里开始。option 就是我们写的代码 例如 {data:{},el:'app',}
  //  this._init(option) // 初始化数据
  // }

  // 在引入 Vue 之时。 这几个函数对Vue做了处理 (在源码的 5075 行)
  // initMixin(Vue); // 初始化
  // stateMixin(Vue); // 状态
  // eventsMixin(Vue); // 事件
  // lifecycleMixin(Vue); // 生命周期, berforeCreate() create() berforeMounted() mounted() 等等这些函数
  // renderMixin(Vue); // 渲染

  // 在 this._init(option) 的时候呢，Vue做了什么呢
  // 首先。简单说两个吧。 首先呢, 会调用生命周期中的 berforeCreate()这个函数。
  // 在 initMixin(Vue) 时,初始化这个函数，没有什么的。就是初始化函数。
  // 讲个重要的吧。 就是 initRender() 这个函数
  // 先给函数 Vue 对象添加这个函数 $createElement (3494行) 这是创建节点的。这是内部实现代码.
  // 有一些其它的操作。但现在不细讲。不是不重要,是太多不利于讲解
  // 现在说下这个函数 function _createElement(){} (3363行) 这个函数就是创建节点
  // 传入了 context,tag,data,children,normalizationType 这五个参数,重点说下前面的三个参数
  // 故名思意 context 内容, tag 类型, data 数据
  // 会生成 一个 vnode 节点。 通常来说是这样的。
  // 这就是传说中的虚拟节点。大家应该都听说过 vue 使用的是虚拟节点。
  // 这就是。为什么使用虚拟节点呢。这是为了更好的做比较。
  // 我讲的都是白话文,不可能不懂。代码就那么多。有些人会说得很多。而且是非常多。就一行代码的事情。你说那么多干嘛啊。
  // 虚拟节点有什么呢。继续往下看
  // 通常呢 代码走到 function createComponent(){} (3434 行) 创建一个组件。
  // 在这之前.判断了传入的 el 是不是 html div 等标签。

  // ----------------------- 上次说到了哪里 回顾一下。 上次呢， 讲到了 render 函数。----------------------------------

  // 现在是 虚拟 dome
  // 其实很多人并不是很了解 虚拟 dom. 这里呢，你就能完完全全的理解了。什么是虚拟的 dom 。 还有 diff 算法。
  // 这与其它的人说法不一样哦。因为 其它人根本就没有讲到点子上。
  // 虚拟 dom 。 与真实的 dom 的区别是这样的。传统的 dom 是很大的。是非常的一个节点。为什么会这样呢。
  // 这就是一个虚拟 dom 的所有属性。看起来好多。但是并真实的 dom 会更多。也就是说 虚拟 dom 比 传统的 dom 更少。
  // this.tag = tag;
  // this.data = data;
  // this.children = children;
  // this.text = text;
  // this.elm = elm;
  // this.ns = undefined;
  // this.context = context;
  // this.fnContext = undefined;
  // this.fnOptions = undefined;
  // this.fnScopeId = undefined;
  // this.key = data && data.key;
  // this.componentOptions = componentOptions;
  // this.componentInstance = undefined;
  // this.parent = undefined;
  // this.raw = false;
  // this.isStatic = false;
  // this.isRootInsert = true;
  // this.isComment = false;
  // this.isCloned = false;
  // this.isOnce = false;
  // this.asyncFactory = asyncFactory;
  // this.asyncMeta = undefined;
  // this.isAsyncPlaceholder = false;




  var vm = new Vue({

  data // 
  props // 
  propsData // 
  computed // 
  methods // 
  watch // 

  silent // 是否 输出警告
  optionMergeStrategies // 选项合并策略
  devtools // 开发工具
  errorHandler // 错误绑定
  warnHandler // 警告绑定
  ignoredElements // 忽略节点
  keyCodes // 键盘按键代码
  performance // 
  productionTip
    

  Vue.extend
  Vue.nextTick
  Vue.set
  Vue.delete
  Vue.directive
  Vue.filter
  Vue.component
  Vue.use
  Vue.mixin
  Vue.compile
  Vue.observable
  Vue.version

  data: function data(){},
  mounted: function mounted(){},

  el: '#app',
  template // 与节点有关
  render // 生命
  renderError

  beforeCreate // 生命周期
  created
  beforeMount
  mounted
  beforeUpdate
  updated
  activated
  deactivated
  beforeDestroy
  destroyed
  errorCaptured



  vm.$data
  vm.$props
  vm.$el
  vm.$options
  vm.$parent
  vm.$root
  vm.$children
  vm.$slots
  vm.$scopedSlots
  vm.$refs
  vm.$isServer
  vm.$attrs
  vm.$listeners

  // vue 上面的指令
  v-text
  v-html
  v-show
  v-if
  v-else
  v-else-if
  v-for
  v-on
  v-bind
  v-model
  v-slot
  v-pre
  v-cloak
  v-once

  // 节点上的属性
  key
  ref
  is
  slot
  slot-scope
  scope
  

  component
  transition
  transition-group
  keep-alive
  slot

  // 实例上面的属性
  vm.$mount
  vm.$forceUpdate
  vm.$nextTick
  vm.$destroy


  vm.$on
  vm.$once
  vm.$off
  vm.$emit
  
  vm.$mount
  vm.$forceUpdate
  vm.$nextTick
  vm.$destroy


  name
  delimiters
  functional
  model
  inheritAttrs
  comments

  // 还有一些方法
  parent
  mixins
  extends
  provide / inject

  })
 </script>
</body>
</html>
```

> 感觉vue就这些，什么都没有了。 属性，接下来呢, 我逐步分析vue。

## 我们编写一个vue的程序是这样的。

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <title></title>
</head>
<body>
  <div id="app" v-if="show">{{ text }}</div>
</body>
<script>
  // 就是这样的简单。稍微写多点的代码吧。这样的话，大家都能理解
  var vm = new Vue({
    el: '#app',
    data: function(){ // 用 es5 的写法吧。告诉你们，es6 的所有语法都可以转换为 es5。 我也不知道推荐哪个好。es6 语法简单。es5 兼容性好。
      return {
        text: '内容'
        show: true
      }
    },
    // 将十大生命周期都写出来吧 一个个将 还是全说呢。全说吧。全部一股脑都说的话。会很恶心的。
    beforeCreate(){ // 现写十大周期还是将vue 所有的代码都写出来呢
        console.group("%c%s","color:red",'beforeCreate--实例创建前状态')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      created() {
        console.group("%c%s","color:red",'created--实例创建完成状态')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      beforeMount() {
        console.group("%c%s","color:red",'beforeMount--挂载之前的状态')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      mounted() {
        console.group("%c%s","color:red",'mounted--已经挂载的状态')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      beforeUpdate(){
        console.group("%c%s","color:red",'beforeUpdate--数据更新前的状态')
        console.log("%c%s","color:blue","el  :"+this.$el.innerHTML)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
        console.log("%c%s","color:green","真实的 DOM 结构:"+document.getElementById('container').innerHTML)
      },
      updated() {
        console.group("%c%s","color:red",'updated--数据更新完成时状态')
        console.log("%c%s","color:blue","el  :"+this.$el.innerHTML)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
        console.log("%c%s","color:green","真实的 DOM 结构:"+document.getElementById('container').innerHTML)
      },
      activated() {
        console.group("%c%s","color:red",'activated-- keep-alive 组件激活时调用')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      deactivated(){
        console.group("%c%s","color:red",'deactivated-- keep-alive 停用时调用')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      beforeDestroy() {
        console.group("%c%s","color:red",'beforeDestroy-- vue实例销毁前的状态')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      destroyed() {
        console.group("%c%s","color:red",'destroyed-- vue实例销毁完成时调用')
        console.log("%c%s","color:blue","el  :"+this.$el)
        console.log(this.$el);
        console.log("%c%s","color:blue","data  :"+this.$data)
        console.log("%c%s","color:blue","message  :"+this.msg)
      },
      methods: {
        changeMsg() {
          this.msg = 'TigerChain111'
        },
        destroy() {
          this.$destroy()
        }
      }
  })
</script>
</html>
```


1. 上次说到哪里了呢。我现在也不知道了。再次开始吗。不用了吧。

2. 我们从实例,结合源码，给你讲解一下源码。从中呢？ 贯彻了什么是 生命周期。什么是虚拟DOM。

3. 估计很多人,都不知道,为什么虚拟DOM diff的算法会比真实的DOM 还要快。就这三个。就这简简单单的三个问题就行。


## 我们这节内容是不需要架构。或者是脚手架的。因为那样你们会走得很远。不要使用脚手架，
## 因为你使用脚手架了之后，你会迷失自我。webpack 等工具啊。这些我都不说。
## 听完这节课你会很舒服的。你认识了底层。原来 vue 是这样玩的。

## 下节课呢? 我们讲解一下。element.js

## 在下节课呢。就是实战了。还是实战总要。你理论再牛逼。实战代码写得跟狗屎一样。

## 绝对有提升。保证不亏。而且老师很幽默。不许叫我老师。叫我大佬。

## 还有 邓小平 同志的一绝话。 实践是检验真理的唯一标准。

### 先说几个简单的。然后再说难的。剩下的抛弃，不一定全是好的，你想浪费生命。就自己去学呗。

### 我决不一定会比 尤雨溪 差。所有的内容都是使用 es5 的语法。

### 为什么用es5的语法。而不是使用 es6 语法。

### es5 更接近于底层。

### 这节课呢。尽是使用中文。不考虑英文。

```html
<!DOCTYPE html>
<html>
<head>
</head>
<body>
  <div id="app">
    <div v-if="show" @click="handleClick">{{ name }}</div>
    <div >{{ otherName }}</div>
    <div v-for="(item, key) in option" :key="key">
      {{ item }}
    <div>
  </div>
</body>
<script src="./vue.js"></script>
<script>
  var vue = new Vue({
    el: '#app',
    data: {
      name: 'lisi',
      show: true,
      option: []
    },
    methods: {
      handleClick: function(){}
    }
    watch:{
      name: function(){}
    },
    compute:{
      otherName: function(){}
    },
    beforeCreate: function(){console.log('这是创建前',this.$el)},
    created: function(){console.log('这是创建后',this.$el)},
    beforeMount: function(){console.log('这是加载前',this.$el)},
    mounted: function(){console.log('这是加载后',this.$el)},
    updated: function(){console.log('这是更新前',this.$el)},
    beforeUpdate: function(){console.log('这是更新后',this.$el)},
    beforeDestroy: function(){console.log('这是更新后',this.$el)},
    destroyed: function(){console.log('这是更新后',this.$el)},
  })
</script>
</html>
```

## 上面应该是一半了

## 是专门针对初学者的,必需是由浅到难

### 此处省略一万个字。

### 大家都应该明白了。接下来休息。不是让你跑。我是让你的大脑休息。不许暂停。

### 我不是什么大神。我也是一个普通人。只是呢,我喝了世界上最毒最毒的鸡汤。

### 坚持就是胜利。下面的课，会比上面的还要痛苦，但是不要怕。因为我给你们打鸡血。

### 我不想浪费你们的时间。时间是宝贵的。还有你们就不要调倍速了。我已经很认真的去做了。

### 不知不觉。你是不是学到了很多的东西。跟我混，有肉吃。当你学完我三个视频的时候。你就真的解脱了。

####  我不是开玩笑的。我套用公式。简单教学。有一个万能的公式。

## 开篇点题。中间重复。结尾点题。一开始说的是 vue 源码。现在懂了吧。diff 比较方法。懂了吧。虚拟 dom 懂了吧。

## 不应用到实战。你们永远都是没有用的。 我将这个代码 全部打包。给你们。必需呢。给我去看源码。

## 接下来呢。就是深入去了解这些事情了。


```html
<!DOCTYPE html>
<html>
<head>
</head>
<body>
  <div id="app">
    <div v-if="show" @click="handleClick">{{ name }}</div>
    <div >{{ otherName }}</div>
    <div v-for="(item, key) in option" :key="key">
      {{ item }}
    <div>
  </div>
</body>
<script src="./vue.js"></script>
<script>
  var vue = new Vue({
    el: '#app',
    data: {
      name: 'lisi',
      show: true,
      option: []
    },
    methods: {
      handleClick: function(){}
    }
    watch:{
      name: function(){}
    },
    compute:{
      otherName: function(){}
    },
    beforeCreate: function(){console.log('这是创建前',this.$el)},
    created: function(){console.log('这是创建后',this.$el)},
    beforeMount: function(){console.log('这是加载前',this.$el)},
    mounted: function(){console.log('这是加载后',this.$el)},
    updated: function(){console.log('这是更新前',this.$el)},
    beforeUpdate: function(){console.log('这是更新后',this.$el)},
    beforeDestroy: function(){console.log('这是更新后',this.$el)},
    destroyed: function(){console.log('这是更新后',this.$el)},
  })
</script>
</html>
```


## 此处省略 一万字。

### 中间我说过。开篇 点题。 中间强化。 最后点题。

### 作业。看完这个 vue 的源码。 没看完的 不许进入下节课。 因为 你的能力没到。下节课上了是在浪费你

### 们的时间。必需做完作业。我的目的是 ， 大家都进步啊。 