---
title: Configuration Management with Spring Cloud Config
tags:
  - Microservices
  - Spring Cloud
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2018-06-26 10:11:01
thumbnail:
---

微服务架构中面临的一个问题就是每个服务有一个配置，随着服务的不断增加，配置文件散落各处，管理不便。Spring Cloud 提供一套中心化配置管理的方案，通过 Spring Cloud Config Server 来对服务提供配置服务。Spring Cloud Config 支持多种管理配置文件的方式，比如环境变量、本地文件、版本控制工具等，相比之下使用版本控制工具来管理更加方便。假如我们使用 github 来管理配置文件，形式如下：
```
/config
    /default
        config-client.yml
    /dev
        config-client-dev.yml
    /prod
        config-client-prod.yml
    /test
        config-client-test.yml
```
default 下的配置文件以应用名称命名，存放公共的配置信息。dev，prod 和 test 分别对应开发，生产和测试的配置文件，各自存放环境差异的配置。Spring Boot 在加载配置文件的时候会先加载 defualt ，然后在加载指定的配置文件，如果有相同的配置，default 中的配置会被覆盖。使用 git 管理配置文件只是我们的第一步，接下来我们看看如何创建 Config Server 。
### **Config Server**
通过声明式注解的方式可以很方便地创建一个 Config Server 。
#### 首先添加 maven 的配置
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```
#### 修改配置文件
```
server:
  port: 3000
spring:
  application:
    name: config-server
  cloud:
    config:
      label: develop # git 分支
      server:
        git:
          uri: https://github.com/bsyonline/microservices # git repo 的地址
          search-paths: config/** # 检索该目录下的配置文件
```
#### 最后给启动类加上 ```@EnableConfigServer``` 注解
```
@EnableConfigServer
@SpringBootApplication
public class ConfigServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }

}
```

Config Server 提供了 REST 的方式来访问，比如 ```/{name}/{profile}/{label}``` 。
* name ${spring.application.name}
* profile ${spring.profiles.active} ，就是环境后缀 dev ， prod 和 test
* label 分支，默认为 master

启动服务访问 [http://localhost:3000/config-client/dev/develop](http://localhost:3000/config-client/dev/develop) 即可看到开发环境的配置信息。

### **Config Client**

接下来看看客户端如何使用 Config Server 来获取配置。
#### 添加 maven 配置
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

#### 添加配置文件
客户端的配置文件有一个特殊的地方需要注意，Config Server 的连接信息要写在 bootstrap.yml 中，不能写在 application.yml 中。原因是 bootstrap.yml 会先于程序启动，这样就可以在程序启动之前加载配置信息。
```
spring:
  cloud:
    config:
      uri:  http://localhost:3000
      label: develop
      name: config-client
      profile: dev
```
>bootstrap.yml 中的内容都是静态的，如果将 bootstrap.yml 打成镜像，就不能灵活修改了，所以 label 和 profile 最好不要写在配置中，而是通过启动参数来控制 ```java -jar config-client-1.0-SNAPSHOT.jar --spring.profiles.active=prod --spring.cloud.config.label=develop --spring.cloud.config.profile=prod``` 。

#### 添加一个访问入口
```
@RestController
public class ConfigClientController {
    
    @Value("${msg: error}")
    String msg;
    
    @GetMapping("/msg")
    public String msg(){
        return msg;
    }
}
```

### 配置更新

我们已经通过 Config Server 和 Config Client 获取配置，但是这是在启动时候加载的，如果配置文件更新，程序是无法动态获取最新的配置的，只有重启之后再次加载配置。那么有没有办法让应用不停机就能获取最新的配置信息呢？答案当然是肯定的。Spring Cloud Config 提供了一个入口可以远程刷新配置，这个入口是隐藏的，需要借助 actuator 才能开启。
#### 首先在 maven 中添加 actuator 配置
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### 在 controller 上添加 ```@RefreshScope``` 注解
```
@RefreshScope
@RestController
public class ConfigClientController {
    ...    
}
```
#### 添加配置信息，打开 actuator 开关
```
management:
  endpoints:
    web:
      exposure:
        include: '*'
```
这样在启动服务的时候可以看到日志中有 ```Mapped "{[/actuator/refresh],methods=[POST]``` 信息。更新配置文件并提交到 git 后，使用 ```curl -XPOST 'http://localhost:3333/actuator/refresh'``` 来刷新配置，完成后再请求应用就会发现获取的配置已经更新了。

