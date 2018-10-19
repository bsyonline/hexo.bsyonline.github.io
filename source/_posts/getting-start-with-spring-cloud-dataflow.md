---
title: Getting Start with Spring Cloud Dataflow
tags:
  - Spring Cloud
  - Microservices
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2018-10-19 11:58:49
thumbnail:
---


### **配置环境**

1. 下载 spring cloud dataflow 的 jar 包
```
wget https://repo.spring.io/release/org/springframework/cloud/spring-cloud-dataflow-server-local/1.6.3.RELEASE/spring-cloud-dataflow-server-local-1.6.3.RELEASE.jar
wget https://repo.spring.io/release/org/springframework/cloud/spring-cloud-dataflow-shell/1.6.3.RELEASE/spring-cloud-dataflow-shell-1.6.3.RELEASE.jar
```

2. 启动服务
```
docker run --name dataflow-rabbit -p 15672:15672 -p 5672:5672 -d rabbitmq:3.7-management
docker run --name dataflow-redis -p 6379:6379 -d redis:3.0
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=scdf -p 3306:3306 -d mysql:5.7
java -jar spring-cloud-dataflow-server-local-1.6.3.RELEASE.jar --spring.datasource.url=jdbc:mysql://localhost:3306/scdf --spring.datasource.username=root --spring.datasource.password=123456 --spring.datasource.driver-class-name=org.mariadb.jdbc.Driver --spring.rabbitmq.host=127.0.0.1 --spring.rabbitmq.port=5672 --spring.rabbitmq.username=guest --spring.rabbitmq.password=guest
```

3. 导入 App

4. 使用 data flow shell
```
    $ java -jar spring-cloud-dataflow-shell-1.6.3.RELEASE.jar 
      ____                              ____ _                __
     / ___| _ __  _ __(_)_ __   __ _   / ___| | ___  _   _  __| |
     \___ \| '_ \| '__| | '_ \ / _` | | |   | |/ _ \| | | |/ _` |
      ___) | |_) | |  | | | | | (_| | | |___| | (_) | |_| | (_| |
     |____/| .__/|_|  |_|_| |_|\__, |  \____|_|\___/ \__,_|\__,_|
      ____ |_|    _          __|___/                 __________
     |  _ \  __ _| |_ __ _  |  ___| | _____      __  \ \ \ \ \ \
     | | | |/ _` | __/ _` | | |_  | |/ _ \ \ /\ / /   \ \ \ \ \ \
     | |_| | (_| | || (_| | |  _| | | (_) \ V  V /    / / / / / /
     |____/ \__,_|\__\__,_| |_|   |_|\___/ \_/\_/    /_/_/_/_/_/

    1.6.3.RELEASE

    Welcome to the Spring Cloud Data Flow shell. For assistance hit TAB or type "help".
    dataflow:>
```
如果出现 ```dataflow-unknown:>``` 需要设置服务器
```
server-unknown:>dataflow config server http://localhost:9393
Shell mode: classic, Server mode: classic
dataflow:>
```

### **官方文档上的 httptest**

1. 创建 stream
```
stream create --name httptest --definition "http | log" --deploy
```

2. 查看 stream
```
dataflow:>stream list
╔═══════════╤═════════════════╤═════════════════════════════════════════╗
║Stream Name│Stream Definition│                 Status                  ║
╠═══════════╪═════════════════╪═════════════════════════════════════════╣
║httptest   │http | log       │The stream has been successfully deployed║
╚═══════════╧═════════════════╧═════════════════════════════════════════╝
```

3. 查看运行的 App 信息
```
dataflow:>runtime apps 
╔════════════════════╤═══════════╤════════════════════════════════════════════════════════════════════════════════════════════════════════════╗
║App Id / Instance Id│Unit Status│                                       No. of Instances / Attributes                                        ║
╠════════════════════╪═══════════╪════════════════════════════════════════════════════════════════════════════════════════════════════════════╣
║httptest.http       │ deployed  │                                                     1                                                      ║
╟┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╢
║                    │           │       guid = 54544                                                                                         ║
║                    │           │        pid = 2424                                                                                          ║
║                    │           │       port = 54544                                                                                         ║
║httptest.http-0     │ deployed  │     stderr = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007655/httptest.http/stderr_0.log║
║                    │           │     stdout = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007655/httptest.http/stdout_0.log║
║                    │           │        url = http://127.0.0.1:54544                                                                        ║
║                    │           │working.dir = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007655/httptest.http             ║
╟────────────────────┼───────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────╢
║httptest.log        │ deployed  │                                                     1                                                      ║
╟┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╢
║                    │           │       guid = 17251                                                                                         ║
║                    │           │        pid = 2406                                                                                          ║
║                    │           │       port = 17251                                                                                         ║
║httptest.log-0      │ deployed  │     stderr = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007348/httptest.log/stderr_0.log ║
║                    │           │     stdout = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007348/httptest.log/stdout_0.log ║
║                    │           │        url = http://127.0.0.1:17251                                                                        ║
║                    │           │working.dir = /tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007348/httptest.log              ║
╚════════════════════╧═══════════╧════════════════════════════════════════════════════════════════════════════════════════════════════════════╝
```

4. 发一条测试数据
```
dataflow:>http post --target http://dataflow-httptest-http.local.pcfdev.io --data "hello world"
> POST (text/plain) http://127.0.0.1:54544 hello world
> 202 ACCEPTED
```

5. 在 ```/tmp/spring-cloud-deployer-69437306715667832/httptest-1539933007348/httptest.log/stdout_0.log``` 中可以看到打印的 hello world 信息。


