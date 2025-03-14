+++
title="故障处理系列-数据清洗堵塞mq"
description="故障处理系列-数据清洗堵塞mq"
tags=["experience"]
categories=["java"]
date="2022-03-15T21:55:00+08:00" 
url="Troubleshooting-Series-Data-Cleaning-and-Blocking-MQ.html"
toc=true
+++
### 1、故障还原

+ 系统负载迅速升高
+ 大量mq的发送被限流，影响到了核心业务（有强依赖mq驱动的）



### 2、根因分析

+ 上游的一个业务触发了系统的数据清洗
+ 清理的数据比较多，清洗的逻辑中需要对外发送大量的广播
+ mq的发送频率过快，大量的超时，进而被熔断
+ 大量的mq无法完成发送
+ 上游大量基于mq驱动的业务受阻

### 3、处理办法

+ 清洗数据为非核心业务，可以控制处理时间、策略。避免对核心业务的冲击

+ 核心业务与非核心业务的mq进行隔离

