+++
title="java内存优化的常见方法"
description="java内存优化的常见方法"
tags=["base"]
categories=["java"]
date="2022-03-15T21:55:00+08:00" 
url="java-memory-optimization.html"
toc=true
+++
## 1、慎用new
![优化无极限](https://static.gzcx.net/oneblog/20210724220825605.png-94rg002)
new就意味着会分配对应的内存空间。利用jdk本身的变量

| 错误              | 建议                   |
| ----------------- | ---------------------- |
| new Boolean(true) | Boolean.TRUE           |
| new Integer()     | Integer.valueOf(int i) |

## 2、String操作

用StringBuffer替换+操作

## 3、容易忽略的细节

| 细节                       | 建议                                     |
| -------------------------- | ---------------------------------------- |
| HashMap                    | 初始化指定大小                           |
| for循环减少变量的计算      | for( inti= 0,len= list.size();i<len;i++) |
| 对象尽量在确定的范围内创建 | if(i== 1){A a = newA();}                 |
| final中及时释放资源        |                                          |
| try cath                   | 不在循环内部使用                         |
| Map遍历                    | 使用Entry操作                            |

## 4、其他的有效的建议

+ 多使用单例
+ 减少static的使用
+ 内部多使用基本的数据类型
+ 尽量使用位运算。int num = a * 4;可改写 intn um = a << 2;
+ HashMap、StringBuffer初始化尽量指定大小。避免扩容复制带来的消耗
+ 无效的局部变量，尽早显示指定null
+ 数组的拷贝尽量使用系统函数 **System.arraycopy ()**
+ 高频使用对用使用缓存
+ 慎用异常控制流程。因为stack track消耗不小
+ try catch


