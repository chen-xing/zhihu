+++
title="git常用的操作"
tags=["git"]
categories=["tool"]
date="2019-12-18T21:55:00+08:00"
url="common-git-operate.html"
toc=true
+++

git常用的操作语法介绍
## 1、回归合并代码的请求

```
git checkout . && git clean -xdf
```



## 2、强制删除本地的代码分支

```
git branch -D branchName
```



## 3、检出代码到本地

```
git checkout -b 本地分支名 远程分支名
```

