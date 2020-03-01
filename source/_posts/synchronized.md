---
title: synchronized
tags:
  - Concurrent
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2020-02-20 09:36:42
thumbnail:
---


synchronized 是 java 关键字，是最常见多线程同步方式，可以作用于方法和代码块。
```
// 同步方法
synchronized void method(){

}

// 同步代码块
synchronized (this) {

}
```
synchronized 具有以下特点：
1. **非公平锁**
2. **可重入**：是指在如果一个线程获得了锁进入了 synchronized 方法，那么该方法内部调用的其他方法可以直接获得该对象的锁。
3. **不可中断**：是指一旦一个线程获得了锁，只有执行完成或抛出异常才能释放，执行过程中其他线程只能等待，直到获得锁的线程释放锁之后其他线程才能获取锁。

synchronized 使用使用时需要注意以下几点：

1. synchronized 代码块的作用的对象不能为空。因为 synchronized 的信息是保存在对象的头信息中，所以作用的对象必须进行实例化。
2. synchronized 虽然简单，但是效率不高，所以在使用是尽可能保证 synchronized 的作用域不能过大。
3. 要注意避免死锁