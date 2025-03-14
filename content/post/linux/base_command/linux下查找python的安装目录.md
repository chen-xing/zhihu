+++
title="linux下查找python的安装目录"
description="linux下查找python的安装目录"
tags=["base command"]
categories=["linux"]
date="2022-03-15T21:55:00+08:00" 
url="Find-the-installation-directory-of-python-under-linux.html"
toc=true
+++
## 1、查找docker

```
docker ps
```

![查找docker容器Id](https://static.gzcx.net/figure_bed/20220223114227.png-94rg002)

## 2、进入docker容器

```
docker exec -it e23686eaed46 /bin/sh 
```



## 3、确定python版本

```
python --version
```

![确定python版本](https://static.gzcx.net/figure_bed/20220223114353.png-94rg002)

## 4、确定pyhon安装目录

首选输入python命令进入python的编码环境。

然后输入下面的打印命令

```
import sys
print (sys.path) 
```
![查找python安装路径](https://static.gzcx.net/figure_bed/20220223114601.png-94rg002)
从截图中看出安装路径是 **/usr/lib/python3.8**
