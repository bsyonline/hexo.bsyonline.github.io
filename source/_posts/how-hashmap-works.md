---
title: HashMap 的工作原理
date: 2016-08-09 11:37:57
tags:
 - Data Structures
category: 
 - Java
thumbnail: 
author: bsyonline
lede: "没有摘要"
---

HashMap 是最常用的集合类之一，在面试中也出镜率颇高。

#### HashMap 和 Hashtable
经常会问 **HashMap 的特点** 及 **HashMap 和 Hashtable 的区别** 等等，那么就先做一下简单总结。

<table style="font-size:12px;color:#333333;border-width: 1px;border-color: #666666;border-collapse: collapse;"><tr><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;"></th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">HashMap</th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">Hashtable</th></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">所在位置</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">实现 Map 接口，JDK 1.2 加入到 Java Collections Framework</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">Hashtable 集成自 Dictionary ，JDK 1.2 实现了 Map 接口</td></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">是否支持 null key 或 null value</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">是</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">否</td></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">线程安全</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">不安全</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">安全</td></tr></table>

以上是3点是我们耳熟能详的二者之间的区别，Hashtable 不是我们此篇的重点，暂且放在一旁。
#### 碰撞
上边这个面试题是通常只是开胃菜，每个熟悉 Java 的人都能知晓。上题热身之后，经常会有这样的问题提出： **HashMap 是如何存储数据的?** 看过源码就能知道 HashMap 底层是使用 **Array** 和 **Linked Table** 来存储数据的。
```java
// jdk 1.8.0_74
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```
HashMap 在 put 一个 key-value 对时，首先对 key 做 hash 操作，然后将 key 分配到 table[i] 的位置。如果两个 key 的 hash(key) 结果相同，那么这两个 key 就都会分配到 table[i] 的位置，这两个 key 就会在 table[i] 的位置以链表的形式存储，这种现象称为 **碰撞**。
可以用一张图来说明，我们假设将取模作为 `hash(key)` 的话，put 如下一组数据：
<table style="font-size:12px;color:#333333;border-width: 1px;border-color: #666666;border-collapse: collapse;"><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">key</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">7</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">17</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">23</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">33</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">39</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">49</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">55</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">73</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">87</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">103</td></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">value</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">a</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">b</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">c</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">d</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">e</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">f</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">g</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">h</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">i</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">j</td></tr></table>
将形成下图的结构
<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20181014/hashmap_01.png" style="width: 600px">
可以看到对 hash(key) 操作后，数据通过链表的形式分别存储在索引 1 和 7 的位置， 17 , 33 , 49 都存储在位置 1 ，就是上文提到的碰撞。如果要从 HashMap 中取到 33 的值，首先需要定位到位置 1 ，再遍历链表。理想情况下，如果在一个 HashMap 中数据都分散在数组中，没有出现碰撞，那么 get 的速度最快，时间复杂度为 O(1) , 如果出现碰撞，get 的性能将有所下降。极端情况下，如果所有数据都存储在相同位置，那 HashMap 就变成了 Linked Table ，时间复杂度为 O(n) 。针对这样的情况，JDK 1.8 对 HashMap 的实现方式进行了优化，如果链表的长度大于阀值，就将链表改成红黑树，get 的时间复杂度为 O(logn) , 如图：
<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20181014/hashmap_02.png" style="width: 300px">
好了，说了这么多，简单总结一下吧。通常情况下，如果两个对象的 key 的 hashCode() 相同，那么他们 bucket 的位置相同而发生碰撞，Map.Entry 对象存储在链表中。当 get 时，通过 hashCode() 找到 bucket 的位置，然后遍历链表，通过 key.equals() 找到指定的值。

**为什么使用 String 作为 key 是一个不错选择？**
其实，答案可以从上边获得。因为，String 是 final 的，并且有固定的 hashCode() 和 equals() 方法，所以能有效减少碰撞的发生，同时由于不可变，可以缓存 key 的 hashcode ，提高 get 对象的速度。同理，如果自定义对象作为 key ，应保证对象是不可变的，即保证 equals() 和 hashCode() 方法正确重写。

#### rehashing 问题
HashMap 还有一个知识点会作为问题提出： HashMap 的容量是固定的么？ 如何扩容？ 会有什么问题？...等等。诸如此类的一系列问题，了解了 HashMap 的 resize 之后都可以轻松回答。
HashMap 有两个重要的参数，DEFAULT_INITIAL_CAPACITY 和 DEFAULT_LOAD_FACTOR 。在初始化时默认容量为16，当 bucket 的使用数量超过 DEFAULT_INITIAL_CAPACITY 和 DEFAULT_LOAD_FACTOR 的乘积时，HashMap 会将容量扩充为原来的两倍（就是将上图中的数组大小变成原来的两倍），同时重新按照 hash(key) 来存放对象到新的数组中，这个过程叫做 rehashing 。
在 JDK 1.8 之前，每次 resize 需要从新计算 hash(key) 的值，在 JDK 1.8 中对这里也做了优化。1.8 的 resize 使用的是 2 次幂扩展，所以在 resize 的过程中，元素要么在原索引位置，要么在2倍索引位置。如下图所示：
<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20181014/hashmap_03.png" style="width: 600px">
看一下源码会更清楚
```java
// jdk 1.8.0_74
Node<K,V> loHead = null, loTail = null;
Node<K,V> hiHead = null, hiTail = null;
Node<K,V> next;
do {
    next = e.next;
    if ((e.hash & oldCap) == 0) {
        if (loTail == null)
            loHead = e;
        else
            loTail.next = e;
        loTail = e;
    }
    else {
        if (hiTail == null)
            hiHead = e;
        else
            hiTail.next = e;
        hiTail = e;
    }
} while ((e = next) != null);
if (loTail != null) {
    loTail.next = null;
    newTab[j] = loHead;
}
if (hiTail != null) {
    hiTail.next = null;
    newTab[j + oldCap] = hiHead;
}
```
先遍历链表算出 hiTail 和 loTail ，然后根据 hiTail 和 loTail 得到元素在新数组中的位置。说简单点，由于扩容是向左移一位，那么不用每次都重新计算 hash(key) ，只要看左边新扩展的一位是 0 还是 1 , 0 就留在原位置，1 就将原位置索引加上原容量得到新位置的索引。计算结果参考下图：
<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20181014/hashmap_04.png" style="width: 850px">
最后 rehashing 有没有问题呢？ 在 jdk 1.8 之前，在多线程环境下，rehashing 会出现条件竞争，导致程序死循环。简单的描述出现死循环的原因是 Map 在扩容时，多个线程会在移动元素的过程出现环形链表，这样使用 ```get()``` 时就会出现死循环，最终导致 cpu 100% ，细节可以看这篇 [https://juejin.im/post/5a66a08d5188253dc3321da0](https://juejin.im/post/5a66a08d5188253dc3321da0)。 不过在 jdk 1.8 以后代码重写，1.8 之前 的 ```resize()``` 方法使用 ```transfer()``` 来移动元素，1.8 以后 ```resize()``` 使用了两组 table 来移动元素，就是上边的那段代码，所以在 1.8 以后这个死循环的问题已经不会出现了，但是还是有可能会出现元素丢失，所以在多线程环境中还是要对 HashMap 进行线程安全处理。
#### concurrentHashMap 和 Hashtable 的区别
HashMap 是线程不安全的，所以为了保证线程安全，我们有多种替代方案： Hashtable 、 Collections.sychroniziedMap() 和 concurrentHashMap 等。我们可以简单的将 Hashtable 理解为 HashMap 的线程安全版本，但是 Hashtable （包括 Collections.sychroniziedMap() ）的性能却不尽如人意，实际使用更多的是 concurrentHashMap 。Hashtable 和 concurrentHashMap 的性能差异的关键是写锁的数量。Hashtable 简单粗暴地使用了整个 Map 范围的锁，插入、删除及检索，甚至遍历等操作都要保持锁，所以极大的限制了并发。concurrentHashMap 则是将锁分散到每个 bucket 。bucket 的锁的数量是 32 ，也就意味着可以有 32 个线程同时操作 Map ，其性能自然比 Hashtable 要好很多。
#### 如何优化 HashMap 的性能
1. 初始化容量
   通常情况下 HashMap 的性能还是比较稳定的，算是在 ArrayList 和 LinkedList 之间取了平衡。

   <table style="font-size:12px;color:#333333;border-width: 1px;border-color: #666666;border-collapse: collapse;"><tr><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;"></th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">获取</th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">查找</th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">添加/删除</th><th style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #dedede;">空间</th></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">ArrayList</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(1)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(1)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N)</td></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">LinkedList</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(1)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(1)</td></tr><tr><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">HashMap</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N/Bucket_size)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N/Bucket_size)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N/Bucket_size)</td><td style="border-width: 1px;padding: 8px;border-style: solid;border-color: #666666;background-color: #ffffff;">O(N)</td></tr></table>
N 为元素的个数，Bucket_size 是桶的数量，在碰撞很少的情况下，复杂度趋近于 O(1) 。如果想进一步提高 HashMap 的性能，则需要在设计时有更多考虑。HashMap 的初始容量为 16 ，容量超过 75% 就会 resizing 。虽然 HashMap 的 resizing 性能再不断提升，但是如果能预估 HashMap 的大小，就能够避免不必要的 resizing 。比如，有 1000 个元素， 下面的写法必定会触发 resizing 。
```java
Map map = new HashMap();
```
或
```java
Map map = new HashMap(1000);
```
不考虑空间因素，2 倍 size 是最简单的方法。
```java
Map map = new HashMap(1000 * 2);
```
但这样并不是太好，因为 HashMap 本身就不省空间。所以靠谱的做法还是自己算一个 init size 。
```java
float size = 1000 / 0.75f;
Map map = new HashMap(size);
```
或
```java
Map map = Maps.newHashMapWithExpectedSize(1000);
```
2. Key 的设计建议
   我们知道 Map 获取元素是通过 Key 来比较的，Integer 的数值型的 Key 可以使用 == 来比较，而 String 需要用 equals ，一个一个字符比较还是比较慢的，好在 String 是 final 的，hashcode 可以缓存，但还是建议在只能使用 equals 的情况下，将 Key 设计的短一些。对于自定义对象类型的 key ，最好还是要重写 hashcode 和 equals 方法。
3. 使用替代数据结构
   在极端情况下，如果 HashMap 的可以使用支持 int 、 long 等原生类型作为 Key 的数据结构来代替。
