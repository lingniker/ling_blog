# 记录 linux 的命令

1. netstat -ntlp // 查看端口的使用 [链接](https://www.cnblogs.com/waylong/p/9132266.html)
2. vim ~/.bash_history // 查看历史版本 [链接](https://blog.csdn.net/Dongguabai/article/details/81159429)
3. lsb_release -a // 查看linux 版本 [链接](https://www.cnblogs.com/wzk-0000/p/7483262.html)
4. lastb // 查看登陆失败日志 [链接](https://blog.csdn.net/weixin_34352449/article/details/87222254)
5. last // 查看登陆成功日志
6. top -p 21538 // top 参看某个进程 [[链接](https://blog.csdn.net/zhangfn2011/article/details/7488746)


### 解决 ssh 长时间不操作， 会自动断开的问题。
```sh
    vim /etc/ssh/sshd_config

    # 修改下面参数
    ClientAliveInterval 30 #客户端每隔30秒向服务发送一个心跳数据
    ClientAliveCountMax 1800 # 客户端多少秒没有相应，服务器自动断掉连接

    # 重启一下服务
    service sshd restart
```
