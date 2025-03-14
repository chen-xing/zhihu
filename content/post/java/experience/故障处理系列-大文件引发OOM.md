+++
title="故障处理系列-大文件引发OOM"
description="故障处理系列-大文件引发OOM"
tags=["experience"]
categories=["java"]
date="2022-03-15T21:55:00+08:00" 
url="Troubleshooting-Series-Large-File-Causes-OOM.html"
toc=true
+++
### 1、场景还原

系统A是一个需要加载文件到内存中进行处理的系统

+ 大量的大文件并发请求过来
+ 收到大量的系统的内存告警
+ 收到大量的5xx告警，同时从监控可以看到大量的fullGC
+ 系统oom
+ 重启，过了3分钟故障再次发生



### 2、原因分析

+ 大量的大文件请求过来，导致系统中内存迅速消耗殆尽
+ 内存不够用触发fullGc
+ 频繁的gc导致系统卡顿，触发5xx
+ gc无法维持内存的稳定，oom
+ 重启无法解决，是因为系统的是通过mq进行驱动的，重启能够是内存恢复，但是mq的重试机制又迅速点燃了这个故障，要想彻底解决，只能是清洗或过滤这一批异常数据

主要原因是系统层面未对这个大文件的场景进行考虑，超出了系统的处理能力。

同时也发现了代码层面的一个坑，**feature**的滥用

+ get 未设置超时时间
+ 异常未cancle

这两个坑将导致系统卡死

正确的做法是：

```
FutureTask<String> future =...;
try {
    	String result = future.get(5000, TimeUnit.MILLISECONDS);
    	System.out.println(result);
	} catch (TimeoutException e) {
		future.cancel(true);
	}

```



### 3、架构考虑

+ 消息队列进行削峰限流，直接rpc的话，系统的性能瓶颈将拖垮整个调用链路
+ 业务打标，大文件、小文件分离处理。
