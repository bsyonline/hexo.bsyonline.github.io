---
title: Developing a Spring Cloud Dataflow App
tags:
  - Spring Cloud
  - Stream Processing
category:
  - Java
author: bsyonline
lede: 没有摘要
date: 2018-10-19 15:47:40
thumbnail:
---



###  pom.xml

```
<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>${commons-lang3.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-stream-test-support</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <distributionManagement>
        <repository>
            <id>mylocal</id>
            <name>Local Repository</name>
            <url>http://localhost:8081/nexus/content/repositories/releases</url>
        </repository>
        <snapshotRepository>
            <id>mylocal</id>
            <name>Local Repository</name>
            <url>http://localhost:8081/nexus/content/repositories/snapshots</url>
        </snapshotRepository>
    </distributionManagement>
```



```
import org.apache.commons.lang3.StringUtils;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Processor;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.handler.annotation.SendTo;

import java.util.Arrays;

@EnableBinding(Processor.class)
@SpringBootApplication
public class ProcessorApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(ProcessorApplication.class, args);
    }
    
    @StreamListener(target = Sink.INPUT)
    @SendTo(Processor.OUTPUT)
    public String split(String s) {
        return StringUtils.join(s.split(","), " ");
    }
}
```



```
dataflow:>app register --name split-processor --type processor --uri maven://com.rolex.microlabs:transform-app:1.0-SNAPSHOT
```



```
stream create --name splittest --definition "splitInput: http | splitTransform: split-processor | splitOutput: file --directory=/tmp --name=split.txt"
```





```
dataflow:>stream deploy --name splittest 
Command failed org.springframework.cloud.dataflow.rest.client.DataFlowClientException: Failed to resolve MavenResource: com.rolex.microlabs:transform-app:jar:1.0-SNAPSHOT. Configured remote repositories: : [springRepo],[repo1]
```



```
--maven.localRepository=mylocal --maven.remote-repositories.repo1.url=http://localhost:8081/nexus/content/repositories/snapshots/
```



```
dataflow:>http post --target http://127.0.0.1:46858 --data "spring,cloud,dataflow"
> POST (text/plain) http://127.0.0.1:46858 spring,cloud,dataflow
> 202 ACCEPTED
```





