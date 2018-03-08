---
title: Oracle Clob 格式处理
toc: false
date: 2016-04-11 15:53:49
tags:
 - Oracle
 - Java
category: 
 - 数据库
thumbnail: 
author: bsyonline
lede: "没有摘要"
---

<!--more-->
```JAVA
try {
    Clob clob = resultSet.getClob("FCT_PARTY");
    if (clob != null && clob.length() > 0) {
        char[] c = new char[(int) clob.length()];
        Reader reader = clob.getCharacterStream();
        reader.read(c);
        fygg.setParty(new String(c));
        reader.close();
    }
} catch (IOException e) {
    logger.error("column {}.FCT_PARTY read failed.", tableName, e);
}
```