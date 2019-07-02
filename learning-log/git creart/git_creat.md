# git 学习 [git官网](https://github.com/)

1. 注册 登录 github (这没什么可说的)

2. 创建一个仓库 点击 new 

  ![创建仓库](./new_repositories.png)

给仓库起一个名字,给个描述,是否是公开的,私人的需要付费。
  
   ![起个名字](./creat_repositories.png)

## 创建了 github 之后，

// git 命令大全
// https://github.com/michaelliao/learngit/blob/master/2019-03/AlexSunShineZhou.md

// git 版本库是不是可已将多人带入进来。
// git fork 请求自己的路径
// git 使用的手册,是什么呢。
// 还有就是将其他人 拉近来有什么用呢。权限的控制。
// 版本的控制,主要的流程还会不知道啊.

```shell
   git clone https://github.com/lingniker/lixi.git //这里是你仓库的地址 将项目下载到本地里 
   cd lixi // 进入到你的仓库中
   git status // 查看库的状态
   git add . // 将所有的修改 条件到暂存区里
   git commit -m 'commit' // 将库由暂存区 提交到本地主干上
   git push // 将库提交到线上
   git check // 切换分支本地没有分支。若果是公开的仓库的话没有必要去创建分支，分支自有你自己知道。
   git merge // 合并分支
   git checkout // 切换分支
   git reset // 版本的回退
   git revert // 版本的回退
   git log // 提交的日记
```

```js
   git init file  // 初始化一个仓库 
```

```js
  git config --global user.name 'lingniker' // 谁是这样库
  git config --global user.mail 'lingyuanping@outlook.com' // 你的邮箱
```

```js
  git add  // 将修改的文件添加到暂存区中
```