# 正则的学习
### 直接上代码吧

```js
  // 正则对象
  // 正则对象上的方法
  //  test exec
  var regExpStr = new RegExp('aa');
  var regExpStr$1 = /a/g;
  regExpStr.test('aa') // true
  regExpStr.test('a') // false
  regExpStr.exec('aaa') // 与 字符的 match 返回的值是一样的。
  // 字符上上的对象 search match 
  var str = 'aaa'
  str.search(/a/); // true 与 test 一致
  str.macth(/a/); // 返回一个数组
  str.indexOf(/a/) // 返回的 -1 进行判断

  // 在字符上还有两个方法 就是 replate split 
  // 这两个方法也是能使用 regExp 的。
  // 大家只要记住第二个就行。

```