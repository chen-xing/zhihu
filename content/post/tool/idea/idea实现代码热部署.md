+++
title="idea实现代码热部署"
description="idea实现代码热部署"
tags=["idea"]
categories=["java"]
date="2022-05-17T21:55:00+08:00" 
url="idea-realizes-code-hot-deployment.html"
toc=true
+++

## Dynamic Code Evolution for Java

动态代码演进虚拟机 (DCE VM) 是对 Java HotSpot VM 的修改，它允许在运行时无限制地重新定义加载的类。HotSpot™ VM 的当前热交换机制只允许更改方法体。增强的 VM 允许添加和删除字段和方法以及更改类的超类型。


1 需要使用 JDK 1.8.0_181 版本，因为 dcevm 需要和 jdk 版本匹配。



2 下载插件：https://github.com/dcevm/dcevm/releases

我下载的是 [DCEVM-8u181-installer-build2.jar](https://github.com/dcevm/dcevm/releases/download/light-jdk8u181%2B2/DCEVM-8u181-installer-build2.jar)。



然后，安装插件。

由于我使用是 mac 版本，我的启动命令是 sudo java -jar DCEVM-8u181-installer-build2.jar 。

启动之后，需要安装 DCEVM；

![DCEVM安装](https://static.gzcx.net/figure_bed/20220518115014.png-94rg002)

点击 Install DCEVM as altjvm， 我这边已经安装成功。



3 idea 下载 **HotSwapAgent** 插件，重启 idea；

开启 插件功能：

![开启热部署插件](https://static.gzcx.net/figure_bed/20220518115109.png-94rg002)

插件源码：https://github.com/HotswapProjects/HotswapAgent



4 springboot 启动命令，加入 -XXaltjvm=DCEVM

![启动参数](https://static.gzcx.net/figure_bed/20220518134756.png-94rg002)

启动后，控制台会输出 hotswap 日志：

![img](https://static.gzcx.net/figure_bed/20220518134856.png-94rg002)





截止到目前，我们就能够实现在 idea 进行热部署 springboot 了。



我们修改代码后，对代码进行重新编译：

![img](https://static.gzcx.net/figure_bed/20220518134927.png-94rg002)



**HotSwapAgent 就会开始工作，会重新刷新 springboot bean 容器：**



![img](https://static.gzcx.net/figure_bed/20220518134950.png-94rg002)



此时，代码就更新完了。