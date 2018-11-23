---
title: Java Garbage Collection
tags:
  - JVM
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2018-08-27 22:20:06
thumbnail:
---



### 垃圾收集算法

#### 标记-清除算法

标记清除算法是最基本的垃圾收集算法。算法分为标记和清除两个阶段。首先，标记出所有需要回收的对象，在标记完后统一回收所有被标记的对象。

标记清楚算法缺点：1. 效率不高；2. 清除后会产生大量不连续的内存碎片，碎片太多，会导致在创建大对象时没有足够的连续内存，会再次出发垃圾回收。

<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20180827/223040708.png" style="width:600px;">


#### 复制算法

复制算法是为了解决标记清除算法的效率问题提出的。复制算法将内存分为大小相等的两份，每次使用其中一份。当其中一块用完了，就将对象复制到另一块上，然后再把原来的内存一次清除。这样每次都对一整块内存进行操作，不会出现内存碎片的问题，简单高效，但是缺点也很明显，就是每次只有一半的内存可以使用。

<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20180827/223717986.png" style="width:600px;">

复制算法在新生代的垃圾回收中被大量采用，新生代被分为 Eden 和 Survivor 。当进行垃圾回收时， Eden 和其中一个 Survivor 的对象会被复制到另一个 Survivor 中。如果 Survivor 空间不够，对象会进行 Old 区，这被称为分配担保（Handle Promotion）。

#### 标记-整理算法

由于复制算法的特点，一般不会在 Old 区使用。根据 Old 区的特点，提出了另一种标记-整理算法。首先对对象进行标记，然后将对象向一端移动，然后清理掉边界以外的内存空间。

<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20180827/224854936.png" style="width:600px;">

#### 分代收集算法

分代收集算法根据对象的存活时间，将内存分为几块：新生代和老年代。根据不同区域的特点选择不同的垃圾回收算法。如新生代中对象创建和死亡的频率很高，所以大多采用复制算法。老年代没有额外的内存进行分配担保，所以大多使用标记-删除和标记-复制算法。

### 垃圾收集器

垃圾收集器是对垃圾收集算法的实现。垃圾收集器可以搭配使用。

<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20180827/225509617.png" style="width:600px;">

#### Serial 收集器

Serial 收集器是单线程的，它会在进行垃圾回收时，会暂停其他所有线程。Serial 收集器简单高效，虽然会造成停顿，但是在 Client 模式下，Serial 收集器是一个不错的选择。

#### Parnew 收集器

Parnew 是 Serial 的多线程版本，是 Server 模式下的新生代收集器首选。其中一个重要原因是，垃圾收集器在使用时有着严格的搭配要求。由上图可以看出，在使用 CMS 作为老年代的垃圾收集器时，新生代的垃圾收集器只有 Serial 和 Parnew 可选。

> 并行和并发
>
> 并行（Parallel）：垃圾收集线程同时工作，用户线程等待。
>
> 并发（Concurrent）：用户线程和垃圾收集线程同时执行，用户线程和垃圾收集线程分别运行在不同的 CPU 上。

#### Parallel Scavenge 收集器

Parallel Scavenge 是新生代收集器，使用复制算法，并行的多线程收集器。Parallel Scavenge 的衡量标准是吞吐量。吞吐量是 CPU 用于运行用户代码与 CPU 总消耗时间的比值。

#### Serial Old 收集器

Serial Old 是 Serial 的连年代版本。

#### Parallel Old 收集器

Parallel Old 是 Parallel Scavenge 。

#### CMS 收集器

CMS 的目标是返回的最小时间。

#### G1 收集器

G1 是面向服务端的收集器。