---
title: Eureka Architecture
tags:
  - Microservices
category:
  - Microservices
author: bsyonline
lede: 没有摘要
date: 2018-08-29 17:07:02
thumbnail:
---

![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180829/210836060.png)

上图为 Eureka 的架构图，从图中可以看到有两类角色：Eureka Server 和 Eureka Client 。Client 又分为 Application Service 和 Application Client 。Eureka Client 通过 API 向 Eureka Server 注册并保持通信，Application Client 通过 API 获取注册信息，并通过远程调用来访问 Application Service 。 