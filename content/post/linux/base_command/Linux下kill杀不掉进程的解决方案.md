+++
title="Linux下kill杀不掉进程的解决方案"
description="Linux下kill杀不掉进程的解决方案"
tags=["base command"]
categories=["linux"]
date="2022-03-15T21:55:00+08:00" 
url="The-solution-to-kill-the-process-under-Linux.html"
toc=true
+++
今天打开Linux虚拟机，然后使用jps命令查看，莫名奇妙多了一个1889进程

![img](https://fastly.jsdelivr.net/gh/chen-xing/figure_bed/images/20201218094240.png)

 然后使用kill杀掉后，再运行jps还是存在此进程。于是乎开始大量百度，最终找到了[解决方案](https://blog.csdn.net/lemontree1945/article/details/79169178)。

 说的很清楚了，杀不掉的原因有两种：

+ 1.这个进程是僵尸进程 
+ 2.此进程是"核心态"进程。

---

- First： 按照方案，我**首先重启**了下看看行不行，结果重启后使用jps命令还是能看到此进程。
- Second：尝试第二种解决方案，**进入到 /proc/1889 目录下**，**执行cat status**，可以看到引用它的**父进程PPID是1584**，于是执行命令**kill -9 1584**就把父进程删除了。最后执行**kill 1889**，然后执行jps就能看到此进程已经彻底Game Over。

![img](https://fastly.jsdelivr.net/gh/chen-xing/figure_bed/images/20201218094246.png)

另外，在kill前如果不放心，怕误杀，可以使用 ls -ail 查看PID被哪个应用程序占用：

![img](https://fastly.jsdelivr.net/gh/chen-xing/figure_bed/images/20201218094248.png)
