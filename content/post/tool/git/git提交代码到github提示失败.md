+++
title="git提交代码到github提示失败"
tags=["git"]
categories=["tool"]
date="2022-02-22T21:55:00+08:00"
url="github-submit-fail.html"
toc=true
+++

## 1、报错信息

```
Connection reset by 20.205.243.166 port 22 fatal: Could not read from remote repository.  Please make sure you have the correct access rights and the repository exists.
```



## 2、处理方案

```
尝试将host文件中添加
192.30.255.112 github.com git
185.31.16.184 github.global.ssl.fastly.net
```



