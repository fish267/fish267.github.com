---
author: Fish
layout: post
title: OpenMP学习笔记<一>
categories: C++
tags: openmp
---
##并行程序设计思路
并行程序设计和串行的稍微不同, 大概经历下面四个阶段:
    <b>划分</b>: 将计算分解成尽可能多的小任务, 可以按处理对象数据进行域分解, 也可以按计算任务进行功能分解. 这两种分解要做到数据集和计算集不相交.
    <b>通信</b>: 划分产生的任务, 一般不能完全独立执行, 需要进行数据通信
    <b>组合</b>: 根据任务的局部性, 将小任务组合成更大的任务
    <b>映射</b>: 将组合后的多个任务分配到多个处理器上, 并期望获得算法的最优性能减少算法的执行时间

#OpenMp 基本概念
OpenMP 属于共享内存编程模型的技术, 通过在源代码中添加指导注释, 成为编译指导: <code> #pragma omp parallel </code>


OpenMP 支持的语言包括 C/C++ 和 Fortran, 支持开源编译器 gcc/g++, 下面首先了解OpenMP 的执行模式和三大要素.

##执行模式
OpenMP 采用 fork-join 的模式, 其中 fork 创建新线程或者唤醒已有的线程, jion 即多线程的会合. 主线程在运行过程中, 需要并行计算的时候, 派生出线程来执行并行任务. 在并行代码执行结束后, 派生线程退出或阻塞, 控制流程回到主线程上. 
##编程要素
###1. 编程指导
在 C/C++ 中, OpenMP 的编译指导指令是以 <code> #pragma omp </code> 开头的, 形式如下:
{% highlight cpp %}
#pragma omp 指令 [子句 [子句]....]
{% endhighlight %}
