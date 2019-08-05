---
title: Destributed Transations
tags:
  - untag
category:
  - uncategory
author: bsyonline
lede: 没有摘要
date: 2019-08-03 22:32:48
thumbnail: 

---


我们通常说的事务是指本地事务，即对一个数据库进行的操作，并且这些操作满足事务的 4 个特性。事务的 4 个特性分别是：
Aotmicity（原子性），Consistency（一致性），Isolation（隔离性），Durability（持久性）。在一个事务中的所有操作要么同时成功，要么同时失败，这是原子性，一旦执行成功 commit 或是执行失败 rollback 那么这个结果必须是持久的。在事务开始和结束中间，存在着中间状态，这些中间状态是对事务之外是隔离的，换句话说，在事务结束之前，对数据进行的任何修改对事务之外都是不可见的，这就是隔离性。在事务结束之后，数据必须是完整并且一致的。

在传统的架构体系中经常使用本地事务来保证数据一致性。但是随着分布式系统的发展，出现了越来越多的跨多个数据库进行的操作，在这种情况下，本地事务就无法保证数据一致性，分布式事务应运而生。分布式事务是针对本地事务来说的，它的目的就是解决在分布式环境下，跨多个数据库操作的数据一致性问题。分布式事务可分为刚性分布式事务和柔性分布式事务。


#### 刚性分布式事务

刚性分布式事务是和本地事务一样，都满足 ACID 特性的，是强一致性的。提到刚性事务，最据代表性的就是 XA 模型。XA 模型是 X/Open 提出的一种分布式事务模型。XA 模型有 3 个组成部分：

​	**AP**（Application Program）：定义事务边界

​    **RM**（Resource Manager）：管理计算机共享资源

​	**TM**（Transation Manager）：负责全局事务，分配事务唯一id，监控事务的执行进度，负责事务的提交，回滚，失败恢复

基于 XA 模型，衍生出多种实现，2PC（two phase commit）就是其标准实现。
<img src="https://raw.githubusercontent.com/bsyonline/pic/master/20190805/2pc.jpg" style="width:400px"/>
2PC 过程如下：

​	第一阶段，AP 发起事务 commit ，TM 发起 prepare 投票，当 RM 都同意后，进行第二阶段。

​    第二阶段，TM 最终执行 commit 。如果 commit 过程出现异常，则根据 recover 进行补偿。

刚性分布式事务的优点是强一致性，但是缺点也很明显，由于要保证数据一致性，那么 TM 要挨个询问 RM 收集投票结果，如果 RM 数量很多，那么在所有 RM 投票完成之前，RM 的资源都是被锁定的，所以会导致全局的资源锁定，处理的性能及其低下。为了提高可用性，出现了柔性分布式事务。

#### 柔性分布式事务

柔性分布式事务和刚性分布式事务不同，其理论基础是 BASE 理论。BASE 是 Basically Available（基本可用），Soft state（软状态）和 Eventually consistent（最终一致性）的缩写，是由 CAP 定理演化而来。

​	**基本可用**是指在特殊情况下，系统可以在功能和性能上保证基本或部分可用，比如系统响应时间从 10ms 降低到 500ms 或者只保证核心服务可用，非核心服务暂时不可用。

​	**软状态**是指在多个服务之间数据存在中间状态，多个服务之间暂时地数据不一致不会影响整体可用。

​	**最终一致性**是指数据经过短暂的不一致，最终能够达到一致状态。

通常柔性分布式事务有以下几种实现：

**TCC**

TCC 是 2PC 的一种变形，是 **T**try-**C**onfirm-**C**ancel 的缩写。TCC 的执行过程和 2PC 类似，首先 try 阶段尝试执行业务，完成资源检查，预留资源。第二阶段，不用进行业务检查，直接进行 confirm ，执行成功，事务结束，如果 confirm 失败，则进行 cancel ，释放 try 阶段预留的资源。

我们通过一个简单的例子来感受一下。假设一个下订单-减库存-支付的场景，各业务方需要针对 TCC 进行改造，比如预留库存，在 try 阶段不能真正扣减库存，所以在数据库里需要增加一个预留库存的字段，再比如支付模块也需要增加字段来预存支付金额。如果 try 阶段预留资源都成功了，那么再将预留字段更新到实际扣减库存字段或扣减金额字段，并清空预留资源字段。一般经过 try 阶段的检查， confirm 基本上都能成功。如果 comfirm 失败了，那么就需要按照预留字段释放资源。通过举例，我们可以看出，TCC 很灵活，但是缺点是和业务耦合性高，因为 try-confirm-cancel 3 个阶段都交由业务方来实现。

**Saga**

Saga 是另一种著名分布式事务模型。1987 年论文 sagas 讲述了一种长事务的处理方案，即 saga 模型。Saga 模型的主要思想就是把一个分布式事务拆分成多个本地事务，每个 saga 子事务 ```T1,T2,...Tn``` 都有对应的补偿模块 ```C1,C2,...Cn-1``` 。当所有子事务都执行成功，那么它的执行顺序是 ```T1,T2,...Tn``` 。如果 Tj 执行失败了，那么它的执行顺序是 ```T1,T2,...Tj,Cj-1,...C2,C1,(0<j<n)``` 。
由于 saga 模型没有 try 阶段，所以当执行失败，需要通过进行恢复。Saga 定义了两种恢复方式：向前恢复和向后恢复。向前恢复就是认为事务一定会执行成功而进行重试。向后恢复就是执行回滚+补偿，实际当中使用较多的是向后恢复。
我们还是以下单-减库存-支付这一场景来进行说明。首先我们需要定义两张事务表，事务状态表和事务调用表，这两张表和业务 DB 是独立的。

事务状态表
<table class="table table-bordered" style="width: 60px"><tr><td>tx_id</td><td>state</td><td>timestamp</td></tr></table>

事务调用表
<table class="table table-bordered" style="width: 100px"><tr><td>tx_id</td><td>action_id</td><td>method</td><td>param_type</td><td>param_value</td></tr></table>

saga 执行过程如下：

1.AP 发起事务后 TM 生成 txId， 向事务状态表存一条记录，状态是执行中 。

<table class="table table-bordered" style="width: 60px"><tr><td>tx_id</td><td>state</td><td>timestamp</td></tr><tr><td>1</td><td>0</td><td>1514736000</td></tr></table>
2.AP 按顺序调用下单-减库存-支付操作，每调用一个操作之前向事务调用表插一条记录。

<table class="table table-bordered" style="width: 100px"><tr><td>tx_id</td><td>action_id</td><td>method</td><td>param_type</td><td>param_value</td></tr><tr><td>1</td><td>1</td><td>/orders</td><td>kv</td><td>orderid=12345</td></tr><tr><td>1</td><td>2</td><td>/stocks</td><td>kv</td><td>stock=-1</td></tr><tr><td>1</td><td>3</td><td>/payment</td><td>kv</td><td>pay=1000</td></tr></table>

3.如果下单-减库存-支付都执行成功，TM 将事务状态表中的记录更新成成功，事务结束。

<table class="table table-bordered" style="width: 60px"><tr><td>tx_id</td><td>state</td><td>timestamp</td></tr><tr><td>1</td><td>1</td><td>1514737000</td></tr></table>

4.如果有一步执行失败，则将事务状态更新成失败，同时立刻通知客户端执行失败。

<table class="table table-bordered" style="width: 60px"><tr><td>tx_id</td><td>state</td><td>timestamp</td></tr><tr><td>1</td><td>2</td><td>1514737000</td></tr></table>

补偿是异步的，所以不用等补偿执行完成再通知客户端。
5.TM 定时扫描事务状态表，如果有失败状态的记录，就按照事务调用表中对应的除最后一步调用记录之外的其他记录调补偿接口进行补偿。

<table class="table table-bordered" style="width: 100px"><tr><td>tx_id</td><td>action_id</td><td>method</td><td>param_type</td><td>param_value</td></tr><tr><td>1</td><td>1</td><td>/orders</td><td>kv</td><td>orderid=12345</td></tr><tr><td>1</td><td>2</td><td>/stocks</td><td>kv</td><td>stock=-1</td></tr><tr><td>1</td><td>3</td><td>/payment</td><td>kv</td><td>pay=1000</td></tr></table>

如果表中有 3 条记录，说明前两条是执行成功的，第 3 条执行失败了。那么只需要执行前两步的补偿，第 3 步是不需要补偿的。那么接下来就按照事务调用表中记录的补偿接口的信息调用 ```/orders?orderid=12345&state=2``` 和 ```/stocks?reduce_stock=1``` 进行补偿就可以了。

6.补偿完成后将事务状态表的状态更新成补偿完成。

<table class="table table-bordered" style="width: 60px"><tr><td>tx_id</td><td>state</td><td>timestamp</td></tr><tr><td>1</td><td>3</td><td>1514739000</td></tr></table>


**异步消息**






