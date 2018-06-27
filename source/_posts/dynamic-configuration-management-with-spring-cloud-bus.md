---
title: Dynamic Configuration Management with Spring Cloud Bus
tags:
  - Microservices
  - Spring Cloud
category:
  - Microservices
author: bsyonline
lede: 没有摘要
date: 2018-06-27 14:45:40
thumbnail:
---

前一节我们使用 Spring Cloud Config 集中管理配置文件，并通过 Config Client 远程刷新。但是这样做有一个明显的问题，就是单独刷新每一个服务会随着服务的数量增加变得越来困难。Spring Cloud 提供了一种消息总线的模式，通过消息中间件把消息路由到各个服务来实现统一刷新服务配置，可以让问题变得简单。
![](http://blog.didispace.com/assets/5-7.png)
使用 Spring Cloud Bus 需要在原来 Spring Cloud Config 的基础上稍作改动。
#### 首先把需要用总线联系的服务中加入 Spring Cloud Bus 的配置
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```
#### 然后加入消息中间件的配置
```
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```
完成后，通过刷新 Config Server 就可以动态更新所有服务配置了。

