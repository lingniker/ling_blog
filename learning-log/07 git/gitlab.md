# 只是草稿 建议不要看

# gitlab 使用
## gitlab 安装过程及注意事项
### gitlab 创建组
#### gitlab 权限的管理 分为 五个权限 
1. 游客
2. 记者
3. 开发者
4. 维护者
5. 创建者 
[详细介绍](https://blog.51cto.com/linuxerxy/1870248)

> 一般来说有只一个仓库的创建者.一个维护者.若干开发者.
1. 创建者拥有最高的权限。用户的添加,删除。仓库的删除等权限。
2. 维护者的权限可以合并代码的。
3. 开发者主要是需求的研发.

## 创建项目组。 new group. 
### 给组起个名字 描述组具体是什么 是公开，还是私有

## 创建项目。在某个组下创建项目（也可以不在组上创建项目）
### new project 就能创建项目了.

## 创建项目就 可以添加开发人员了
### 一般来说开发人员是没有权限去 提交到 版本库上的。
### 在这里 可以给开发人员创建一个分支。
### 开发人员只需 关注 dev 下的项目就可以。

### git 的配置
### ssh 配置

```bash
   ## 使用的是 rsa 加密方法
   ssh-keygen -t rsa -C "haiyan.xu.vip@gmail.com"
   ## 输入名称 gitlab_id_rsa
   ## 输入密码 123456
   ### 会生成两个文件 gitlab_id_rsa gitlab_id_rsa.pub

   ### 将 gitlab_id_rsa.pub 上传到gitlab 上 ssh-key
   ### ssh-agent 启动代理
   ssh-agent bash
   ### 将ssh添加到链接上
   ssh-add ~/.ssh/gitlab_id_rsa
```

## 就可以将项目从 服务器上 clone 到 本地上

```bash
  ## 将项目clone本地上
  ## clone 其实 执行了 git remote add origin git@192.168.0.94:test/test.git  将其与源相关联了
  git clnoe git@192.168.0.94:test/test.git
```

## 分支的创建 

```bash
  ## 创建分支
  git branch dev
  ## 分支的查看 
  git branch -a
  ## 分支的切换 
  git checkout dev
  ## 远程创建分支 这样就能在远程创建分支了
  git push origin dev 
```

## 开发者并不能提交 到 master 主干版本。
## 将项目 clone 下来之后。切换分支 git checkout dev 
### 获取代码 git pull origin dev // 都是在这个分支上修改
### 获取 git push origin dev

## 特别注意的地方
1. https://www.cnblogs.com/lihow/p/8903361.html

2. HEAD detached from 处于分离的状态。为什么会处于分离的状态呢。

3. 切换tag 的时候会处于分离的状态

git rebase、git checkout

// 关于tag 分支的管理。
由于在 tag 分支上的代码是不可以改动的。tag 是游离状态的。
在 tag 的基础上创建一个分支。

```bash
  # 在 tag 的基础上创建分支
  git branch -b new_branch v1.0
  # 在分支上做修改后
  git add .
  git commit -m '紧急修复bug'
  # 发布第二版
  git tag v1.1.1 
  # 不能在 tag 分支上修改 只能创建一个分支去修改
```

1. 解决冲突的问题
```js
  // 都会有冲突
  // 冲突的产生是什么呢？ 大家修改了同一个文件的某一行
  // 在 pull 的时候会自动的 merge 合并代码
  // git 会提示合并的内容。
  // 跟修改同一代码的人商量,根据实际情况将冲突的地方合并就行。
``` 


2. 版本的 回滚 回退
[参考链接](https://blog.csdn.net/yxlshk/article/details/79944535)
```bash
  # 回退 git 是前进的。 使用 git reset 是将代码回退到某个版本上。 这样在 某个版本之后的代码都不会存在。
  git reset
  ## 解决的办法是 让 所有的人将在本地上创建一个分支。将分支上的代码。等回退提交完成之后将代码。提交上来。
  ## 将代码 前进一步 。之前提交的将会被保留 将
  git revert 
```

> 管理权限有了 版本控制有了 代码管理有了 冲突解决有了 工作流程有了 整理一下就行了 还有一个 ssh-key 的配置
> 暂告一段落。 