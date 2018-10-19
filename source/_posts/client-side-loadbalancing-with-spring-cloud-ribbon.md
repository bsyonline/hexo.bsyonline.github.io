---
title: Client Side Loadbalancing with Spring Cloud Ribbon
tags:
  - Microservices
  - Spring Cloud
  - Ribbon
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2018-06-30 19:47:19
thumbnail:
---

Spring Cloud Ribbon 是 Netflix 开源的组件之一，主要功能是提供客户端的负载均衡。 Spring Cloud 和 Ribbon 的集成使得实现负载均衡变得非常容易。我们先实现一个简单的例子，由一个客户端在两个服务器之间实现负载均衡。
#### 首先，加入 maven 依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```
#### 客户端调用服务端
```
@RestController
public class RibbonClient1Controller {

    Logger logger = LoggerFactory.getLogger(getClass());

    @Autowired
    LoadBalancerClient loadBalancerClient;
    @Autowired
    RestTemplate restTemplate;

    @GetMapping("/hello")
    public String hello() {
        logger.info("uri {} called", "/hello");
        ServiceInstance serviceInstance = loadBalancerClient.choose("ribbon-server");
        String url = "http://" + serviceInstance.getHost() + ":" + serviceInstance.getPort() + "/ribbon";
        logger.info("url : {}", url);
        return restTemplate.getForObject(url, String.class);
    }

}
```
服务端就是普通的 Spring Boot 程序，启动多个服务，通过访问客户端可以发现请求轮流发送给服务端。
如果使用声明式注解，上边的代码也可以这样写：
```
@RibbonClient(name = "ribbon-server")
@RestController
public class RibbonClient1Controller {

    Logger logger = LoggerFactory.getLogger(getClass());

    @Autowired
    RestTemplate restTemplate;

    @GetMapping("/hello")
    public String hello() {
        logger.info("uri {} called", "/hello");
        return restTemplate.getForObject("http://ribbon-server/ribbon", String.class);
    }

    @LoadBalanced
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```
```@RibbonClient``` 注解还可以指定 ribbon 的自定义配置来替换默认配置。
```
@RibbonClient(name = "ribbon-server", configuration = RibbonConfiguration.class)
public class RibbonClient1Controller {

}
```
比如修改负载均衡的算法：
```
public class RibbonConfiguration {
 
    @Autowired
    IClientConfig ribbonClientConfig;
 
    @Bean
    public IPing ribbonPing(IClientConfig config) {
        return new PingUrl();
    }
 
    @Bean
    public IRule ribbonRule(IClientConfig config) {
        return new RandomRule();
    }
}
```
提供了两个接口，IPing 用来通过 ping 服务器的 url 来判断服务是否可用，IRule 设置负载均衡的规则。