---
title: 面试问题
tags:
  - Interview
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2019-11-07 19:46:00
thumbnail:
---



Q: 为什么使用微服务？

A: 1. 解耦，减少人员的冲突，提高开发和测试的效率。2. 更好地做CI/CD。

Q: 为什么用rabbitmq？

A: 主要对比 kafka。1. 使用场景可靠性更高。2. 延时消息。3. Kafka 优势是高吞吐，目前的业务使用rabbitmq可以满足。4. 社区活跃，资料多。5. 界面友好。

Q: 怎么保证rabbitmq消息不丢？

A: 1. 生产者一端，使用 ack 机制，同时mq开启事务消息，事务成功才返回 ack，事务失败则重试。2. rabbitmq 持久化，等落盘之后再返回ack。3. 消费端手动ack。

Q: 接口的幂等性是怎么设计的，怎么保证幂等？

A: 1. 请求幂等，将数据库增删改操作的相对值修改改为绝对值修改。2. 使用全局id。3. 业务上幂等，比如同一个用户在 10 分钟内不能多次添加相同名单。

Q: 用过哪些 linux 命令？

A: 常用的命令有：查看cpu情况 top，查看内存情况 free， 查看网络情况 netstat，查看磁盘 df ，还有一些 java 的命令，jps， jstat ，jinfo， jstack 。 

Q: Threadlocal 的使用场景

A: 

Q: 如何设计一个系统

A: benchMark 对标：了解公司和业内的先进的解决方案，进行对标。

​     efficiency 效率：原型，可重用的系统或组件，自动化的工具等等。

​     architecture 架构：简单，合适，可演进。

​     function 功能：梳理需求，了解业务，整理出功能。

​     quality 质量：一方面功能要可用，可测试；另一方面要做好兜底方案，保证系统的稳定性。

​     performance 性能：根据业务场景进行设计和优化，常用的手段有无状态化有利于动态扩容缩容，串行改并行，单线程改多线程，同步改异步，读写分离，分库分表，sql优化，jvm参数优化等。

​     safety 安全：1. 权限控制；2. sql注入，各种网络攻击等；3. 协议或数据加密。

Q: 面试经典算法：二分查找，快速排序，反转链表，遍历二叉树，给定两束之和求下标，最近公共子节点，爬楼梯算法

A: 

Q: linux 常用命令，awk，sed，sort，uniq，统计文本中出现次数最多的ip

A: 

Q: 如何定位和排查问题

A: 1. 先从日志入手，判断是否是业务问题。2.是否是资源不足，可借助top，free，jstack等。3. 定位之后制定短期和长期解决方案。

Q: redis的数据结构

A: 

Q: mysql update的执行过程

A: 

