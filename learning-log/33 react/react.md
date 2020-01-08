1. react 原理
一切皆原生。
任何的框架 vue react 要变成原生 html css js。
会发现 react 使用 innerHTML appendChild 原生属性在页面添加节点。
Vue 也是如此。
Angular 应该如此(没接触过)



jsx 语法.
```jsx
    
    // 这是 jsx 语法
    const title = <h1 className="title">Hello, world!</h1>;;

    // 会被转化为下面这个 这是 js 认识的语(babel 处理后就不会存在jsx语法了)
    const title = React.createElement(
        'h1',
        { className: 'title' },
        'Hello, world!'
    );

```
[可以尝试下]](https://babeljs.io/repl) https://babeljs.io/repl


接下来就是处理js事情了 不在讨论jsx, 如果在项目中 碰到jsx 语法。都可以转化为js。
 

```js
  // 一般会应用这两个组件
  import React, { Component , createElement } from 'react';
  import ReactDom from 'react-dom';

  React.createClass() // es5 创建组件的方法(不细讲)

  // 组件的三种创建办法 class function {}
  class A extends React.Component { // 这是 es6 的语法糖. es5 并没有 class
  }

  function B () {
  }

  var C = {
    'div'
  }

  // 使用这个创建节点
  React.createElement('div', null , 'clild')
  

  // Component 是一构造函数
  // 原型链上 存在 setState 这个属性
  Component.prototype.setState = function(){}
  // 有一个属性 state
  // 创建节点
  // createElement()
```

class       // 使用 类
function // 
还有就是 对象  
{
   type: {}
   props: {}
}





react 每个组件都能单独使用 。而vue 着像是。要挂载到 vue compoment 上。vue 自带组件。