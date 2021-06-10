### 关于 es6 与 es5 的转换。

### es6 通过 babel-loader 转化后的到的代码
### 

```js
  // es6
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
    eat() {
      return  'eat'
    }
    static say() {
      return 'say'
    }
  }

  // 通过babel转换成的es5语法
  "use strict";
  // 判断某对象是否为某构造器的实例
  function _instanceof(left, right) { 
    if (right != null && typeof Symbol !== "undefined" && right[Symbol.hasInstance]) {
      return !!right[Symbol.hasInstance](left); } else { return left instanceof right; }
  }

  // 检查声明的class类是否通过new的方式调用，否则会报错
  function _classCallCheck(instance, Constructor) { if (!_instanceof(instance, Constructor)) { 
    throw new TypeError("Cannot call a class as a function"); } 
  }

  /**
  *将方法添加到原型上，如果是静态方法添加到构造函数上，
  **/ 
  function _defineProperties(target, props) { 
    // 遍历函数数组，分别声明其描述符 并添加到对应的对象上
    for (var i = 0; i < props.length; i++) { 
      var descriptor = props[i];
      descriptor.enumerable = descriptor.enumerable || false; // 设置该属性是否能够出现在对象的枚举属性中。默认为 false
      descriptor.configurable = true; // 设置该属性描述符能够被改变，同时该属性也能从对应的对象上被删除。
      if ("value" in descriptor) descriptor.writable = true; // 如果属性中存在value, value设置为可以改变。
      Object.defineProperty(target, descriptor.key, descriptor); // 写入对应的对象上
    }
  }

  // 收集公有函数和静态方法，将方法添加到构造函数或构造函数的原型中，并返回构造函数。
  function _createClass(Constructor, protoProps, staticProps) { 
    if (protoProps) _defineProperties(Constructor.prototype, protoProps); // 共有方法写在property原型上
    if (staticProps) _defineProperties(Constructor, staticProps); // 静态方法写到构造函数上 
    return Constructor; 
  }

  var Person = function () {
    function Person(name, age) {
      _classCallCheck(this, Person);

      this.name = name;
      this.age = age;
    }

    _createClass(Person, [{
      key: "eat",
      value: function eat() {
        return 'eat';
      }
    }], [{
      key: "say",
      value: function say() {
        return 'say';
      }
    }]);
    return Person;
  }();
```

```js
  // 抽象语法树(Abstract Syntax Tree, AST);
  // const add = (a, b) => a + b;
  {
    "type": "Program",
    "body": [
      {
        "type": "VariableDeclaration", // 变量声明
        "declarations": [ // 具体声明
          {
            "type": "VariableDeclarator", // 变量声明
            "id": {
              "type": "Identifier", // 标识符（最基础的）
              "name": "add" // 函数名
            },
            "init": {
              "type": "ArrowFunctionExpression", // 箭头函数
              "id": null,
              "expression": true,
              "generator": false,
              "params": [ // 参数
                {
                  "type": "Identifier",
                  "name": "a"
                },
                {
                  "type": "Identifier",
                  "name": "b"
                }
              ],
              "body": { // 函数体
                "type": "BinaryExpression", // 二项式
                "left": { // 二项式左边
                  "type": "Identifier",
                  "name": "a"
                },
                "operator": "+", // 二项式运算符
                "right": { // 二项式右边
                  "type": "Identifier",
                  "name": "b"
                }
              }
            }
          }
        ],
        "kind": "const"
      }
    ],
    "sourceType": "module"
  }
  // Parse(解析)
  // Transform(转换)
  // Generate(代码生成)
```

```js
  // Parse(解析)
  // 词法分析
  // [
  //     { "type": "Keyword", "value": "const" },
  //     { "type": "Identifier", "value": "add" },
  //     { "type": "Punctuator", "value": "=" },
  //     { "type": "Punctuator", "value": "(" },
  //     { "type": "Identifier", "value": "a" },
  //     { "type": "Punctuator", "value": "," },
  //     { "type": "Identifier", "value": "b" },
  //     { "type": "Punctuator", "value": ")" },
  //     { "type": "Punctuator", "value": "=>" },
  //     { "type": "Identifier", "value": "a" },
  //     { "type": "Punctuator", "value": "+" },
  //     { "type": "Identifier", "value": "b" }
  // ]
```

```js
  /**
    * 词法分析 tokenize
    * @param {string} code JavaScript 代码
    * @return {Array} token
    */
  function tokenize(code) {
    if (!code || code.length === 0) {
        return [];
    }
    var current = 0; // 记录位置
    var tokens = []; // 定义一个空的 token 数组

    var LETTERS = /[a-zA-Z\$\_]/i;
    var KEYWORDS = /const/; //  模拟一下判断是不是关键字
    var WHITESPACE = /\s/;
    var PARENS = /\(|\)/;
    var NUMBERS = /[0-9]/;
    var OPERATORS = /[+*/-]/;
    var PUNCTUATORS = /[~!@#$%^&*()/\|,.<>?"';:_+-=\[\]{}]/;

    // 从第一个字符开始遍历
    while (current < code.length) {
        var char = code[current];
        // 判断空格
        if (WHITESPACE.test(char)) {
          current++;
          continue;
        }
        // 判断连续字符
        if (LETTERS.test(char)) {
            var value = '';
            var type = 'Identifier';
            while (char && LETTERS.test(char)) {
                value += char;
                char = code[++current];
            }
            // 判断是否是关键字
            if (KEYWORDS.test(value)) {
                type = 'Keyword'
            }
            tokens.push({
                type: type,
                value: value
            });
            continue;
        }
        // 判断小括号
        if (PARENS.test(char)) {
            tokens.push({
              type: 'Paren',
              value: char
            });
            current++;
            continue;
        }
        // 判断连续数字
        if (NUMBERS.test(char)) {
          var value = '';
          while (char && NUMBERS.test(char)) {
            value += char;
            char = code[++current];
          }
          tokens.push({
            type: 'Number',
            value: value
          });
          continue;
        }
        // 判断运算符
        if (OPERATORS.test(char)) {
            tokens.push({
                type: 'Operator',
                value: char
            });
            current++;
            continue;
        }
        // 判断箭头函数
        if (PUNCTUATORS.test(char)) {
            var value = char;
            var type = 'Punctuator';
            var temp = code[++current];
            if (temp === '>') {
                type = 'ArrowFunction';
                value += temp;
                current ++;
            }
            tokens.push({
                type: type,
                value: value
            });
            continue;
        }
        tokens.push({
            type: 'Identifier',
            value: char
        });
        current++;
    }
    return tokens;
  }
```


```js
    // 语法分析
    // Transform(转换)
    // 将 tokens 转化为 AST
    // 通过 parse 将 tokens 转化为 抽象树AST
```

```js
  // Generate(代码生成)
  // 根据抽象树, 成功解析为 es5 代码
```

```js
  // 再来一次 再次生成token
  // 再次生成抽象树 AST
```

```js
  // 语义分析 
  // Ignition解释器将AST编译成字节码 Bytecode
  // 生成字节码
```

```js
  // TurboFan：编译器
  // 将 Bytecode 转换为优化的汇编代码
```

```js
  // 最后由底层去解析
  // 二进制的机器代码
```

```js
  // 汇编语言入门教程
  // http://www.ruanyifeng.com/blog/2018/01/assembly-language-primer.html
```
