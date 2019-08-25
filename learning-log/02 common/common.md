# 这是一个普通的日记
### 这记录着，我遇到过的问题，没有规则

1. 截屏工具 使用 FSCapture ,已备份到 百度云上     (20190628)
2. 离线状态 使用qq 截屏 [qq截屏](https://jingyan.baidu.com/article/91f5db1bf55dc61c7e05e352.html) (20190628)
3. tar -cf file.tar file // linux shell
4. pm2 show id // pm2  
5. pm2 list // pm2 展示所有的进程
6. netstat -ntlp // 查看端口的使用 [连接](https://www.cnblogs.com/waylong/p/9132266.html)

> 2019.06.30 

7. filter 数组函数, 
```js
arr.filter(function(item){ // 不会改变原始数组。
  return true // 函数中返回 true 的数组
})
```

// 学到了一个知识点 匿名函数能传参
```js
    var b=5;
    var a = (function(){
        var i = 1;
        return b+i;
    })(b);
    console.log(a);//a=6
    // 这么
```