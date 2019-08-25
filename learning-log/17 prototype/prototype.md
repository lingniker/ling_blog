# javascript 基础之原型和原型链

## 原型
> [prototype](https://user-gold-cdn.xitu.io/2019/4/29/16a6788d36a61f0b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
* 每个 **函数** 都有 `prototype` 对象属性,除了 `Function.prototype.bind()`, 它指向原型。
* 每个 **对象** 都有 `__proto__` 对象, 它指向创建这个对象的构造函数的原型,其实这个属性指向了 `[prototype]`, 但是 `[prototype]`是内部属性,我们访问不到, 所以用 `__proto__` 来访问。
* 对象可以通过 `__proto__` 来寻找部署该对象上的属性, `__proto__`将对象连接起来组成 **原型链**。

小结: 从上图可以看出, **一切皆对象,对象都有 `__proto__` 属性**

## 1.通过 `new` 出来的对象,其 `__proto__` (这个对象的属性) 指向创建这个对象的构造函数的原型;而这个构造函数的原型也是对象，它的 `__proto__` 指向 `Object` 的原型;
```js
  function Foo(){}
  const f1 = new Foo();
  f1.__proto__  => Foo.prototype;
  Foo.prototype.__proto__ => Object.prototype;
  Object.prototype.__proto__ => null // 到头了
```
## 2. 那我们平常用的字面上对象呢？
```js
  const obj = {};
  obj.__proto__ => Object.prototype;
  Object.prototype => null // 到头了

  const arr = [];
  arr.__proto__ => Array.prototype;
  Array.prototype.__proto__ => Object.prototype;
  Object.prototype.__proto__ => null // 到头了

```
## 3. 既然一切皆对象，那函数的 `__proto__` 是怎样的呢？ 它指向构造函数 `Function` 的原型;

```js
  function fn() {}
  fn.__proty__ => Function.prototype;
  Function.prototype.__proto__ => Object.prototype;
  Object.prototype.__proto__ => null; // 到头了
```

## 4. 函数都有 `prototype` 属性，它的 constructor 又指回函数自己
```js
  fn.prototype.constructor => fn;
```

# new 

* 新生成一个对象 `let obj = new Object`;
* 获取构造函数  `let Con = [].shift.call(arguments)`;
* 连接到原型  `object.__proto__ = Con.prototype`;
* 绑定this `let result = Con.apply(obj, arguments)`;
* 返回新对象 `return typeog result === 'object' ? result : obj`;

在调用 `new` 的过程中会发生以上 4 件事:

```js
  function create () {
    // 创建一个空的对象
    let obj = new object();
    // 获取构造函数
    let Con = [].shift.call(arguments);
    // 连接到原型
    obj.__proto__ = Con.prototype;
    // 绑定 this, 执行构造函数
    let result = Con.apply(obj,arguments);
    // 确保 new 出来的是个对象
    return typeof result === 'object' ? result : obj;
  }
```

对于实例化对象来说，都是通过 new 产生的, 无论是 `function Foo()` 还是 `let x = { y: 1 }`。

对于创建一个对象来说,推荐使用**字面量**的方式创建对象(无论性能害死可读行)。因为你使用 `new Object` 的方法创建对象需要通过作用域链一层层找到 `Object`,但是你使用字面量的方法就没这个问题了。
```js
  function Foo() {
    // function 就是一个语法糖 内部等同与 new Function()

    let x = {y : 1};
    // 这个字面量内部也是 使用了 new Object();
  }
```
对于 `new` 来说，还需注意下运算符优先级

```js
  function Foo () {
    return this;
  }
  Foo.getName = function () {
    console.log('name')
  }
  Foo.prototype.getname = function() {
    console.log('prototype name');
  }
  new Foo.getName(); // name
  new Foo().getName(); // prototype name
```

`new Foo()` 的优先级高于 `new Foo`,所以上面的最后2行代码可划分执行顺序

```js
  new (Foo.getName());
  (new Foo()).getName();
```

# instanceof
`instanceof` 可正确判断对象的类型，因为内部机制是通过判断对象的原型链中是不是找到类型的 `prototype` 。 下面自我实现一个 `instanceof` 函数

```js
  function inof (left , right) {
    // 获取类型的原型
    right = right.prototype;
    // 获取对象上的原型
    left = left.__proto__;
    // 判断对象的类型是否等于类型的原型
    while (true) {
      if (left === null) {
        return false
      }
      if (right == left) {
        return true
      }
      left = left.__proto__
    }
  }
```
测试一下

```js
function fn() {}
  inof(fn, Function); true
  inof(fn, Object); true
```

# this 

他是一个会让很多人混淆的概念，让我们用几个场景规划来记住它。

``` js
  // 场景一
  function fn() {
    console.log(this.a);
  }
  var a = 1;
  fn(); // 1

  // 场景二
  var obj = {
    a: 2,
    fn: fn
  }
  obj.fn(); // 2
  // 以上两种场景 `this` 依赖于调用函数前的对象

  // 场景三 - 优先级最高, `this` 只会绑定在 `obj1` 上, 不会被任何方式修改 `this` 指向
  var obj1 = new fn();
  obj1.a = 3;
  console.log(obj1.a); // 3
  
  //场景四 - 利用 call,apply,bind 改变 this,这个优先级仅次于 new
  fn.call(obj); // 2 
```

以上 4中场景弄清楚了，很多代码中的 this 就不是问题了。

```js
  function fn (){
    return () => {
      return () => {
        console.log(this);
      }
    }
  }
  console.log(fn()()()); // window
```

箭头函数中其实是没有 `this` 的, 这个函数中的 `this`只取决于它外面的第一个**不是箭头函数的函数**的`this`。在这个例子中,因为调用 `fn` 符合前面代码中的第一个场景，所以 `this` 是 `window`。 并且 `this`一旦绑定了上下文,就不会被任何代码改变。