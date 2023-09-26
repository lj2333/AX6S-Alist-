# AX6S-Alist-
首先，如果是一台全新的AX6S的话，需要解锁ssh，通过Xshell,或者terminus等软件登录。此步骤不再赘述，网络上有很多教程。推荐“酱紫表”的博客，有详细的操作步骤。

解锁ssh后，通过winscp工具，连接到路由器的文件系统。

在data文件夹下，新建一个Alist文件夹，方便后期管理。

在alist 的[release](https://github.com/alist-org/alist/releases)页面里，下载[alist-linux-musl-arm64.tar.gz](https://github.com/alist-org/alist/releases/download/v3.28.0/alist-linux-musl-arm64.tar.gz)版本。

先下载完成后，将gz压缩包通过winscp传到Alist文件夹中。

打开ssh，输入登录密码。看到 root@xiaoqiang：的抬头之后敲击如下代码。

```
#索引目录
cd /data/Alist

#解压，这一步可能会因为路由器内存不足，无法解压，需要自行找解决方法。实测一台全新的路由器的内存是够的。
tar -zxvf alist-linux-musl-arm64.tar.gz

# 授予程序执行权限
chmod +x alist

# 运行程序
./alist server
```


输入如上命令之后，alist就运行成功了。 但是此时alist只是运行了，如果关闭ssh软件，alist就会立即退出。需要让他持续运行就需要添加一个脚本文件。

在Alist文件夹中，新建一个alist.sh文件。 并在文件中，粘贴如下代码：

```
cd /data/Alist/;./alist admin
AlistPID=`ps -w | grep alist | grep -v grep | awk '{print $1}'`
if [ "$AlistPID" != "" ];then killall alist;fi;sleep 1;
start-stop-daemon -Sbc root -x ./alist -- server
```
然后在ssh工具中，在sh文件所在的文件夹，将这个脚本执行就可以。

以上步骤完成后，alist就正常运行在路由器中了，如果路由器没有定期重启的习惯，就可以不用管他。

如果路由器经常需要重启，则需要添加一个开机自启动脚本。因为我自己的路由器不经常重启，所以没往下进行，有需要可以自行摸索。
