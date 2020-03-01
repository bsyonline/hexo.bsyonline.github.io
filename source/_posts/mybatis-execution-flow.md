---
title: Mybatis Execution Flow
tags:
  - MyBatis
category:
  - MyBatis
author: bsyonline
lede: 没有摘要
date: 2020-02-03 10:54:04
thumbnail:
---

1. 实例化 org.mybatis.spring.SqlSessionFactoryBean 。
2. 通过 buildSqlSessionFactory() 构造 Configuration ，解析 xml 将 statement ，resultMap 等信息加入到 context ，同时将 sql 进行绑定放到 SqlSource 。
3. org.apache.ibatis.builder.MapperBuilderAssistant.addMappedStatement 将 context 中的 statement 信息构造成 MappedStatement ，放到 Configuration 。
4. 创建查询接口 mapper 的代理对象 org.apache.ibatis.binding.MapperProxy ，通过“包名+类名+方法名”从 Map 中取出对应的 sql 语句执行。