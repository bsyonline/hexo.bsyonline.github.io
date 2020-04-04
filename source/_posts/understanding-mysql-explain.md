---
title: Understanding MySQL Explain
tags:
  - Interview
category:
  - MySQL
author: bsyonline
lede: 没有摘要
date: 2020-04-01 20:25:15
thumbnail:
---

Explain 会展示 MySQL 优化器关于语句执行计划的信息，也就是说会解释 MySQL 将如何处理语句。
我们先来试一下。
```
mysql> explain select * from t_user where name='tom';
+----+-------------+--------+------------+------+---------------+--------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys | key    | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+---------------+--------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | t_user | NULL       | ref  | i_name        | i_name | 202     | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+---------------+--------+---------+-------+------+----------+-------+
```

Explain 结果显示了 12 列（黑体是我们需要重点关注的列）。
1. id：序号。
2. **select_type**：select 的类型。
3. table：这行记录属于哪个表。
4. partitions：匹配的分区。
5. **type**：查找的类型。
6. possible_keys：可能选择的索引。
7. key：实际选择的索引。
8. key_len：选择的索引的长度。
9. **ref**：与索引比较的列。
10. rows：读取的行数。
11. filtered：按条件过滤的行数占比。
12. **Extra**：附加信息。 

##### select_type
select_type 有以下一些值：
1. SIMPLE：简单SELECT（不使用 UNION 或子查询）。
2. PRIMARY：最外层 SELECT 。
3. UNION：UNION 中第二个及以后的 SELECT 语句。
4. DEPENDENT UNION：UNION 中的第二个及以后的 SELECT 语句，依赖外层查询。
5. UNION RESULT：UNION 的结果。
6. SUBQUERY：子查询中第一个 SELECT 语句。
7. DEPENDENT SUBQUERY：子查询中第一个 SELECT 语句，依赖外部查询。
8. DERIVED：derived 表
9. MATERIALIZED：
10. UNCACHEABLE SUBQUERY：子查询结果无法缓存，必须针对外部查询的每一行重新进行评估。
11. UNCACHEABLE UNION：UNION 中的第二个及以后的 SELETE 语句，并且这个 UNION 属于 UNCACHEABLE SUBQUERY 。

##### type
1. system：该表只有一行记录。
2. const：全表最多只有一行匹配。
```
mysql> explain select * from t_user where id=1;
+----+-------------+--------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | t_user | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
```
3. eq_ref：A 表中的每一行，B 表中只有一行匹配。（存疑）
4. ref：
5. fulltext：使用 fulltext 索引进行连接。
6. ref_or_null：类似 ref ，但是还会搜索包含 NULL 的行。
```
mysql> explain select * from t_user where skill='aaa' or skill is null;
+----+-------------+--------+------------+-------------+---------------+---------+---------+-------+------+----------+-----------------------+
| id | select_type | table  | partitions | type        | possible_keys | key     | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+--------+------------+-------------+---------------+---------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | t_user | NULL       | ref_or_null | i_skill       | i_skill | 43      | const |    2 |   100.00 | Using index condition |
+----+-------------+--------+------------+-------------+---------------+---------+---------+-------+------+----------+-----------------------+
```
7. index_merge：
```
mysql> explain select * from t_user where name='user1' or skill is null;
+----+-------------+--------+------------+-------------+----------------+----------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table  | partitions | type        | possible_keys  | key            | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+--------+------------+-------------+----------------+----------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | t_user | NULL       | index_merge | i_name,i_skill | i_name,i_skill | 202,43  | NULL |    2 |   100.00 | Using union(i_name,i_skill); Using where |
+----+-------------+--------+------------+-------------+----------------+----------------+---------+------+------+----------+------------------------------------------+
```
8. unique_subquery：
9. index_subquery：
10. range：
11. index：
12. ALL

##### Extra
1. 
2. const row not found
3. Deleting all rows
4. Distinct
5. FirstMatch