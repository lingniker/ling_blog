# webpack 的学习
## 问题 webpack 是什么, webpack 怎么用, webpack 的使用细节, webpack 如何打包。常用的插件。webpack 能用来干嘛。
## 接解决的办法。

## [官网地址](https://www.webpackjs.com/guides/getting-started/) https://www.webpackjs.com/guides/getting-started/


## babel 做了什么
### 在 webpack 中是如何加载，文件的。浏览器并不会认识 sass 文件
```js
  // webpack 的配置文件 可能是这样的 
  var webpackConfig = {
    entry: 'index.html',
    output: 'output.html',
    module: { // 模块
      rules: [ // 定义规则
        {
          test: /.ts/, // 任务
          loader: 'eslint-loader', // 使用的是 loader
        }
      ]
    }
  }
```

```js

// ts 文件
const NUM = 45

interface Cat{
  name: String,
  sex:String
}

function touchCat(cat:Cat) {
  console.log(cat.name)
}

touchCat({
  name:'tom',
  sex:'man'
})

// 通过一定得规则 生成这个

// js 文件

var NUM = 45;
function touchCat(cat) {
    console.log(cat.name);
}
touchCat({
    name: 'tom',
    sex: 'man'
});

```

1. 最简单例子就是这样子

```js
// 这就是最简单的例子了
module.exports = function (res) {
  // res 是传进的字符串
  var a = res.replace(/`/g,'\\`') // 这是做相应的处理
  return `module.exports = function(){
    return \`${a}\`
  }`
  // 这是最后的生成的数据 这就react 的语法了
  // module.exports = function(){
  //  return  '' 
  // }
}
// 大概就是这个思路
// 可以想想 webpack 如何解析 jsx 的
// 进入 jsx 
// 需要传一个回调函数
```

```js

```

