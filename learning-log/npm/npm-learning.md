# npm 包的管理
1. npm发布一个包 [学习](https://www.jianshu.com/p/289212f74804)
2. 查看所有全局安装过的包
3. 较全的文档 [npm菜鸟](https://www.runoob.com/nodejs/nodejs-npm.html)

4. cmpn 设置淘宝镜像  `npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 镜像的切换
1.得到原本的镜像地址

`npm get registry `

> https://registry.npmjs.org/

  设成淘宝的

  `npm config set registry http://registry.npm.taobao.org/`
  

2.换成原来的

  `npm config set registry https://registry.npmjs.org/`
