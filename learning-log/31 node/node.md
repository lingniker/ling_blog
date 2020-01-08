# nodejs 的学习

## [官方文档](https://nodejs.org/en/)
## [中文文档](http://nodejs.cn/api/)
## [node github](https://github.com/nodejs/node)

## nodejs是双版本为稳定版本 现在最稳定的版本的 v10.16.3 (LTS) node v12.11.0 官方并没有将其更改为最稳定版本 现在node 最新的的版本的是 v13.x 这并不是会成为支持的版本。

对于nodejs 单线程是如何工作的。node 如何解决高并发的。

既然nodejs。不利于处理计算密集型。那么就交给 c语言和pyhton吧。
现在最主要的事情就是将。nodejs 与 c 语言相结合。
如果这样能行的话。那么nodejs 就是无敌的。
因为这样 我们就可以复用很多很多的轮子。
而并不是自己在去造轮子了。

libuv 是什么。介绍一下

api 的事情。官网上那么详细。自己去看吧。

大概说几个核心的。
fs 源码分析。
http 源码分析。
path 源码分析。


v8 引擎是什么，为什么能集成到 nodejs上. 
什么是非阻塞I/O.
node 缺点 单线程.  
是如何实现异步编程的。

自己实现一个简单的node-demo 整体思路
