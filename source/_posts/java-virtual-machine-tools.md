---
title: Java Virtual Machine Tools
tags:
  - untag
category:
  - uncategory
author: bsyonline
lede: 没有摘要
date: 2018-09-12 10:14:48
thumbnail:
---



### jps

jps (JVM Process Status Tool) 列出正在运行的 JVM 的进程，以及执行主类的名称和进程的本地虚拟机 id 。jps 通常是查看 jvm 信息的第一步。

jps 的使用示例：

```
$ jps
4611 SubApplication
25782 Main
7415 Jps
4504 RemoteMavenServer
4072 Main
24617 EurekaServerApplication
26297 ZuulApplication
25835 Main
```

| option | desc               |
| ------ | ------------------ |
| -q     | 只输出进程 ID           |
| -l     | 输出主类的全名            |
| -v     | 输出启动时的 JVM 参数      |
| -m     | 输出启动是传入 main() 的参数 |

### jstat

### jstack

### jinfo

### jmap

### HSDIS

### JConsole

### VirtualVM

