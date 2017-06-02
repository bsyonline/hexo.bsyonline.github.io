---
title: Spark 调度管理
date: 2017-05-31 11:50:47
tags:
---



### Spark Application 之间的调度

每个 Spark 程序会获得一个独立的 JVM 来执行任务和存储数据。当多个 Spark 程序共享集群资源，不同的集群管理器会有不同的调度策略。

**静态资源分配**

最简单的方式是静态划分所有可用的集群资源。这种方式，每个 Spark 程序被分配它可用的最大资源，并在整个声明周期都占有这些资源。这种方式在 Spark Standalone 、YARN 和 [coarse-grained Mesos](https://spark.apache.org/docs/1.6.3/running-on-mesos.html#mesos-run-modes) 模式中。

* Standalone mode 模式默认提交的任务是按照 FIFO 的顺序执行，执行期间占有所有可用的节点。通过修改 `spark.cores.max` 和 `spark.executor.memory` 参数可以修改程序可使用的节点数和内存。
* YARN 模式通过设置 `--num-executors` 选项控制 Executor 的数量，使用 `--executor-memory` 和 `--executor-cores` 选项控制 Executor 可用的资源。

> 不管使用何种方式，都不能在程序之间共享内存。

**动态资源分配**

Spark 提供了一种按照负载动态分配资源的方式，当程序不在使用资源的时候将资源还给集群，这种特性在多个程序共享集群资源的时候是非常有用的。这个特性是默认关闭的，可以在所有 coarse-grained cluster managers 上设置启用。

Spark 程序可在等待被调度的任务被挂起的时候动态请求额外的 Executor 。Spark 以轮询的方式请求 Executor ，在任务被挂起到达一定时间时，请求才真正触发。每次触发请求的 Executor 数量以指数递增。这样做的原因是考虑到应用在开始阶段只需要较少的 Executor ，当确实需要大量的 Executor 时，指数递增能够快速提升处理能力。

相对来时 Spark 的释放策略要简单的多，当闲置超过一定时间 Executor 会被释放。在大多是情况下，任务被挂起说明资源不足，闲置说明资源过剩，所以 Executor 的申请和释放是互斥的。

静态分配资源时，相关的程序正常退出或出错，Executor 都会退出，两种情况下和 Executor 相关的状态不会再被使用，可以安全的移除。但是动态分配资源时，Executor 退出时程序还在运行，如果程序访问该 Executor 保存的状态，就会重新计算。比如，在 shuffle 期间，Spark 先将 map 写到本地磁盘，然后其他 Executor 可以访问计算结果。在 shuffle 完成之前，Executor 会被移除，完成后，shuffle 的结果将不可访问，如果其他的 Executor 要访问，就需要重新计算。因此，Spark 提供了额外的 shuffle 服务来保存 shuffle 文件，即使 Executor 退出，也可以通过该服务访问 shuffle 文件，因为 shuffle 服务是独立于程序和 Executor 的。

### Application 内的调度

**调度逻辑**

Spark 是按照 DAG 图来进行的任务调度的。调度可分为两部分： DAGScheduler 和 TaskScheduler 。DAGScheduler 负责任务的逻辑调度， TaskScheduler 负责任务的物理调度。

DAGScheduler 首先需要逆向遍历 RDD 依赖链，划分调度 stage ，确定 stage 之间的依赖关系。Spark 通过 ShuffleDependency 来划分 stage，对应 ShuffleDependency 的 RDD 叫做 ShuffleRDD，stage 的边界是从 ShuffleRDD 的父 RDD 计算的。

**调度策略**

在 Spark 程序内部，多个线程提交的任务可以并行执行，是线程安全的。默认的 Spark 任务调度策略是 FIFO 。每个任务被划分到 stage 中，stage 开始时第一个任务获得优先权，然后是第二个，以此类推。如果一个任务没有使用全部资源，下一个任务可以立刻开始执行。如果前边的任务占用了全部的资源，后边的任务只能等待前边的任务完成后再开始执行。

Spark 可以通过配置 `spark.scheduler.mode`  为 FAIR 来让任务公平共享资源。在公平模式下，在任务长时间运行时新提交一个较小的任务也能够马上开始执行，不需要等待之前的任务执行完成再开始执行。和 Hadoop Fair scheduler 类似，公平调用支持将作业分组到调度池中，并且为每个调度池分配不同的选项，比如将重要的任务放在优先级高的池中或是将不同用户的任务放在各自的池中。调度池有 3 个配置选项：

* schedulingMode 可配置任务在池内的调度策略。
* weight 可配置池的获得的资源，即优先级。
* minShare 可配置池的最小资源。

默认情况下，池的属性为 `scheduling mode FIFO weight 1 minShare 0` 。

