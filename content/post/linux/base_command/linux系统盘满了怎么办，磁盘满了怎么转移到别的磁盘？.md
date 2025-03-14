+++
title="linux系统盘满了怎么办，磁盘满了怎么转移到别的磁盘？"
description="linux系统盘满了怎么办，磁盘满了怎么转移到别的磁盘？"
tags=["base command"]
categories=["linux"]
date="2022-03-15T21:55:00+08:00" 
url="What-should-I-do-if-the-linux-system-disk-is-full.html"
toc=true
+++
## 1、现象

使用docker部署了一个蓝眼网盘，正常运行了一段时间，今天突然无法访问了。起初还以为是docker进程因为某种原因down机了，或者因为网络策略无法访问服务端口。

## 2、排查步骤

+ 上服务器curl服务主页，同样提示无法访问，可以锁定是服务的问题
+ 尝试重启docker容器,提示磁盘满了

```
Error response from daemon: Cannot restart container 54e6a5481d99: mkdir /var/lib/docker/overlay2/24ebc597e84c2cc642116e7b6363df5dd1f2fa5c1f13d805efb41a55d5408242/merged: no space left on device
```

+ 查看磁盘占用情况

  ![查看磁盘占用情况](https://static.gzcx.net/figure_bed/20220228164200.png-94rg002)

可以看到的是系统盘使用已经是100%了，

+ 结合自己的应用类型(网盘)，磁盘占用量比较大
+ 由于是使用docker，默认存储的路径是固定的。所以能做的是：
  + 修改docker应用的默认存储位置
  + 将文件挂载在添加盘，然后用软连接的方式进行关联

## 3、修改docker容器挂载目录

```
systemctl stop docker.service（关键，修改之前必须停止docker服务）

vim /var/lib/docker/containers/container-ID/config.v2.json

systemctl start docker.service

docker start <container-name/ID>
```



## 4、软链接实现存储转移

### 4.1、创建软链接

```
mv /chenxing /data/tank

ln -s /data/tank/chenxing /usr/local
```

+ 不存在目标目录才可以创建软链接！
+ 实际目录在前，快捷方式在后



### 4.2、删除软连接

rm –rf 软链接名称（**请注意不要在后面加”/”，rm –rf 后面加不加”/” 的区别，可自行去百度下啊**）

**切记不要自动补全删除，如果是rm -rf test/ 那么原目录下的文件都会被删除！！！**



### 4.3、查看软链接

```
ls -il
```

![软链接查看](https://static.gzcx.net/figure_bed/20220228174948.png-94rg002)
