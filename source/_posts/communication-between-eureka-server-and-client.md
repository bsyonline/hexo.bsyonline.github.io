---
title: Communication between Eureka Server and Client
tags:
  - Microservices
category:
  - Microservices
author: bsyonline
lede: 没有摘要
date: 2018-08-29 17:06:28
thumbnail:
---





Eureka Server 和 Eureka Client 之间是通过一组 API 进行通信的，包括服务注册（registry），续约（renew），退出（cancel）和剔除（evict）。

### 注册

Eureka Client 启动之后首先会向 Eureka Server 进行注册，注册流程如下：

![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/213336520.png)

1. Client 通過 ```POST /eureka/v2/apps/{appID}``` 向 Server 发起注册请求；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/194055677.png)

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/193909570.png)

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/195210819.png)

2. Server 收到请求后，用双层 HashMap 保存注册信息；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/202843068.png)

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203115942.png)

3. 更新自我保护机制阈值 ```expectedNumberOfRenewsPerMin``` 和 ```numberOfRenewsPerMinThreshold``` ;

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203204566.png)

   > - `expectedNumberOfRenewsPerMin` ，期望**最大**每分钟**续租**次数。
   > - `numberOfRenewsPerMinThreshold` ，期望**最小**每分钟**续租**次数。

4. 将服务名字放到 ```recentRegisteredQueue``` 中；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203238017.png)

5. 更新实例的状态；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203428268.png)

6. 将实例信息放到 ```recentlyChangedQueue``` 中；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203518017.png)

7. 通过 ```invalidate()``` 清除 guava 缓存；

   ![mark](http://7xqgix.com1.z0.glb.clouddn.com/blog/20180902/203555642.png)

8. 向相邻节点复制信息。

### 续约

