---
title: Services Communication using Feign
tags:
  - Microservices
  - Feign
  - Spring Cloud
category:
  - Microservices
author: bsyonline
lede: 没有摘要
date: 2018-06-20 16:22:09
thumbnail:
---

在服务注册到 Eureka 后，可以通过 Eureka 获取服务地址，使用 http 可以进行通信，这和我们使用 httpclient 、jeseryclient 或 restTemplate 的方式一样。但是还有一种更为简单的方式，就是 Feign 。Feign 是 Spring Cloud 的组件之一，是一个声明式的 REST 客户端。

#### **使用声明式的 Feign 注解**
Feign 内部集成了 Ribbon ，使用起来要简单很多。比如我们在 api-gateway 中使用过滤器时对请求进行身份认证，需要访问 user-center 来获取用户信息。假定我们已有 user-center 。
```
@RestController
public class UserController {
    
    Logger logger = LoggerFactory.getLogger(getClass());
    
    @GetMapping("/find_user_by_client_id")
    public User find(String clientId) {
        logger.info(String.format("invoke {}.{}(clintId={})", "UserController", "find", clientId));
        
        if ("1".equals(clientId)) {
            return new User("1", "1", "tom");
        }
        return null;
    }
}
```
这就是一个普通的服务。那么如何在 api-gateway 中使用 feign 来进行通信呢？
##### 1. 在的 api-gateway 中添加 maven 配置
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

##### 2. 创建一个 feign 客户端
```
@FeignClient(name="user-center")
@Service
public interface UserService {

    @GetMapping(value = "/find_user_by_client_id")
    User findUserByClientId(@RequestParam("clientId")String clientId);

}
```
请求的路径和服务端一致。

##### 3. 在启动类添加注解 ```@EnableFeignClients```
```
@EnableFeignClients
@EnableZuulProxy
@EnableDiscoveryClient
@SpringBootApplication
public class ApiGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }

}
```
##### 4. 在过滤器中调用 feign 客户端
```
@Component
public class AccessFilter extends ZuulFilter {

    private static Logger logger = LoggerFactory.getLogger(AccessFilter.class);

    @Override
    public String filterType() {
        return "pre";
    }

    @Override
    public int filterOrder() {
        return 0;
    }

    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Autowired
    UserService userService;

    @Override
    public Object run() {
        RequestContext ctx = RequestContext.getCurrentContext();
        HttpServletRequest request = ctx.getRequest();
        logger.info(String.format("%s >>> %s", request.getMethod(), request.getRequestURL().toString()));

        String clientId = request.getParameter("clientId");
        if(isEmpty(clientId)){
            logger.warn("client id is required");
            ctx.setSendZuulResponse(false);
            ctx.setResponseStatusCode(401);
            try {
                ctx.getResponse().getWriter().write("client id is required");
            } catch (Exception e) {
            }
            return null;
        }
        
        if (userService.findUserByClientId(clientId) == null) {
            logger.warn("user is not existed");
            ctx.setSendZuulResponse(false);
            ctx.setResponseStatusCode(401);
            try {
                ctx.getResponse().getWriter().write("user is not existed");
            } catch (Exception e) {
            }
            return null;
        }
        logger.info("ok");
        return null;
    }
}
```
可以看到，比我们使用 httpclient 等方式要简洁很多。

#### **Feign 的其他用法**
如果想定制 Feign 的功能，可以使用 FeignClientsConfiguration 。FeignClientsConfiguration 可以复写 ```feign.Decoder```、```feign.Encoder``` 和 ```feign.Contract``` ，并在 ```@FeignClient``` 中添加属性。
```
@FeignClient(name = "stores", configuration = FooConfiguration.class)
public interface StoreClient {
    //..
}
```