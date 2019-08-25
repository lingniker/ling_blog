# 部署文档
1. 简单介绍下, 在阿里申请服务器。 Ubuntu 18 版本。
2. 登录服务器，下载 nvm (node 包管理工具)
```shell
   apt install nvm
```
3. 使用 nvm 下载 nodejs
```shell
  nvm install node
```
4. 安装 git
```bash
  apt-get install git
```
5. 下载项目 
```bash
  git clone <url> // 下载项目
  cd  <name>      //项目下
  npm install 
```
6. 安装 pm2 包 
```bash
  npm install pm2 -g
  pm2 start
  打开浏览器,完成ok
```