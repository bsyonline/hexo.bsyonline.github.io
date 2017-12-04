---
title: Oracle 分页 sql 模板
toc: false
date: 2016-05-05 15:53:54
tags:
 - untag
category: 
 - uncategory
thumbnail: 
author: bsyonline
lede: "没有留下前言"
---

Oracle通过ROWNUM进行分页，通过子查询实现。

```sql
SELECT *
FROM (SELECT
        A.*,
        ROWNUM RN
      FROM (
        SELECT * FORM t
      ) A
      WHERE ROWNUM <= 100)
WHERE RN >= 0
```
