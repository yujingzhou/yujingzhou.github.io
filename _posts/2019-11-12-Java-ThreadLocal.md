---
layout: post
title: ThreadLocal
category: Java
tags: [Java]
---

ThreadLocal

## 分为如下几点阐述：

1. ThreadLocal是什么
2. ThreadLocal数据结构
3. 深入解析ThreadLocal类
4. ThreadLocal应用场景

## ThreadLocal是什么

ThreadLocal是一个本地线程副本变量工具类。主要用于将私有线程和该线程存放的副本对象做一个映射，各个线程之间的变量互不干扰，在高并发场景下，可以实现无状态的调用，特别适用于各个线程依赖不通的变量值完成操作的场景。

## ThreadLocal数据结构

下图为ThreadLocal的内部结构图
![ThreadLocal的内部结构图](https://upload-images.jianshu.io/upload_images/7432604-ad2ff581127ba8cc.jpg)

从上面的结构图，我们已经窥见ThreadLocal的核心机制：

* 每个Thread线程内部都有一个Map。
* Map里面存储线程本地对象（key）和线程的变量副本（value）
* Thread内部的Map是由ThreadLocal维护的，由ThreadLocal负责向map获取和设置线程的变量值。

所以对于不同的线程，每次获取副本值时，别的线程并不能获取到当前线程的副本值，形成了副本的隔离，互不干扰。

## 深入解析ThreadLocal

ThreadLocal类提供如下几个核心方法：
```
public T get()
public void set(T value)
public void remove()
```

* get()方法用于获取当前线程的副本变量值。
* set()方法用于保存当前线程的副本变量值。
* initialValue()为当前线程初始副本变量值。
* remove()方法移除当前前程的副本变量值。

ThreadLocalMap的问题
由于ThreadLocalMap的key是弱引用，而Value是强引用。这就导致了一个问题，ThreadLocal在没有外部对象强引用时，发生GC时弱引用Key会被回收，而Value不会回收，如果创建ThreadLocal的线程一直持续运行，那么这个Entry对象中的value就有可能一直得不到回收，发生内存泄露。

如何避免泄漏
既然Key是弱引用，那么我们要做的事，就是在调用ThreadLocal的get()、set()方法时完成后再调用remove方法，将Entry节点和Map的引用关系移除，这样整个Entry对象在GC Roots分析后就变成不可达了，下次GC的时候就可以被回收。

如果使用ThreadLocal的set方法之后，没有显示的调用remove方法，就有可能发生内存泄露，所以养成良好的编程习惯十分重要，使用完ThreadLocal之后，记得调用remove方法。

## ThreadLocal应用场景

最常见的ThreadLocal使用场景为 用来解决 数据库连接、Session管理等

* 每个ThreadLocal只能保存一个变量副本，如果想要上线一个线程能够保存多个副本以上，就需要创建多个ThreadLocal。
* ThreadLocal内部的ThreadLocalMap键为弱引用，会有内存泄漏的风险。
* 适用于无状态，副本变量独立后不影响业务逻辑的高并发场景。如果如果业务逻辑强依赖于副本变量，则不适合用ThreadLocal解决，需要另寻解决方案。



