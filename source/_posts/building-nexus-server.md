---
title: Nexus 私服搭建
date: 2016-02-22 15:53:46
tags:
 - Git
category: 
 - Java
thumbnail: 
author: bsyonline
lede: "没有摘要"
---


### 下载

下载最新的Nexus OSS

```
wget http://www.sonatype.org/downloads/nexus-latest-bundle.tar.gz
```

### 解压
```
tar -zxf nexus-latest-bundle.tar.gz  
mv nexus-2.11.1-01 nexus2

chown -R nexus2
chown -R sonatype-work
```
### 启动
```
cd nexus2
./bin/nexus start
```
### 安装服务

修改 ```bin/nexus``` 的 ```RUN_AS_USER=root```

创建服务

```
ln -s /usr/nexus2/bin/nexus /etc/init.d/nexus
source /etc/profile
```

启动服务

```
service nexus start
```

开机启动

```
chkconfig --add nexus  
chkconfig --levels 345 nexus on  
```
### 访问

在浏览器中输入[http://192.168.1.201:8081/nexus/](http://192.168.1.201:8081/nexus/)
![img](https://raw.githubusercontent.com/bsyonline/pic/master/20181014/9.png)

### 配置

管理员账号admin，默认密码admin123，通过右上角登录。

### 添加代理仓库

点击左侧的Repositories，然后再点击右侧的Add，会弹出下拉菜单，选择Proxy Repository，如下配置。
![https://raw.githubusercontent.com/bsyonline/pic/master/10.png](https://raw.githubusercontent.com/bsyonline/pic/master/20181014/10.png)
选中Public Repositories，将新建的Proxy Repository加入到Public Repositories组，并Update Index。
![https://raw.githubusercontent.com/bsyonline/pic/master/11.png](https://raw.githubusercontent.com/bsyonline/pic/master/20181014/11.png)

### 配置maven

修改maven的**settings.xml**文件
```
<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <localRepository>D:\Dev\mvn\repository</localRepository>
    <pluginGroups>
        <pluginGroup>org.mortbay.jetty</pluginGroup>
    </pluginGroups>
    <proxies>
    </proxies>

    <!--设置 Nexus 认证信息-->
    <servers>
        <server>
            <id>nexus-releases</id>
            <username>admin</username>
            <password>admin123</password>
        </server>
        <server>
            <id>nexus-snapshots</id>
            <username>admin</username>
            <password>admin123</password>
        </server>
    </servers>
    <!--设置 Nexus 镜像，后面只要本地没对应的以来，则到 Nexus 去找-->
    <mirrors>
        <mirror>
            <id>nexus-releases</id>
            <mirrorOf>*</mirrorOf>
            <url>http://192.168.0.201:8081/nexus/content/groups/public</url>
        </mirror>
        <mirror>
            <id>nexus-snapshots</id>
            <mirrorOf>*</mirrorOf>
            <url>http://192.168.0.201:8081/nexus/content/groups/public-snapshots</url>
        </mirror>
    </mirrors>
    <profiles>
        <profile>
            <id>local</id>
            <properties>
                <downloadSources>true</downloadSources>
                <downloadJavadocs>true</downloadJavadocs>
            </properties>
            <repositories>
                <repository>
                    <id>local</id>
                    <url>http://192.168.0.201:8081/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>local</id>
                    <url>http://192.168.0.201:8081/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>daily</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>local</activeProfile>
    </activeProfiles>
</settings>
```

第一次安装后，nexus私服仓库是空的，通过maven下载jar包显示从私服仓库下载，实际还是从公网下载，然后保存到本地,以后再下载相同的jar包就直接从本地下载了。
