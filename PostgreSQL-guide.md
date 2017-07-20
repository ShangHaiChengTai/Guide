# PostgreSQL Guide

---	
## PostgreSQL规范

---

## 文档目录
0. [背景](#0-背景)
1. [设计规范](#1-设计规范)
2. [操作规范](#2-操作规范)
3. [备注](#3-备注)

---
## 0 背景
PostgreSQL功能异常强大，希望通过以下规范，使PostgreSQL的功能以及性能得以充分的发挥，为应用层提供快准狠的数据服务。持续完善中，悉心接受各路意见和建议。


## 1 设计规范

### 1.1 数据库

1. 为数据库访问账号设置复杂密码。

2. 数据库的命名由一个单词组成，且均为小写，不要以postgre或者pg开头，禁止使用[SQL关键字](https://www.postgresql.org/docs/current/static/sql-keywords-appendix.html)。


### 1.2 数据表

1. 表的命名为名词的复数形式，且均为小写，不要以postgre或者pg开头。如果表名由几个单词构成，单词之间用下划线("_")连接。

2. 表名长度限制在30字符以内。表名尽量使用全名。当长度超过30位时，可使用缩写来缩减长度。

3. 建议每张表都有主键，且采用以下的写法：id serial primary key或者id bigserial primary key。

4. 临时备份的数据表，建议在名称后加上日期（tabel\_XXX_YYYYMMDD）

5. 对于频繁更新的表，将填充因子(fillfactor)设置成85，每页预留15%的空间给HOT更新使用（备注1）。


### 1.3 数据表字段

1. 表字段的命名为有意义的单词或是单词的缩写，且均为小写。如果字段名由几个单词构成，单词之间用下划线("_")连接。

2. 表字段名长度限制在30字符以内。

3. 建议能用varchar(N)就不用char(N),以节省存储空间。

4. 建议能用varchar(N)就不用text,varchar。

5. 建议使用default NULL，而不用default ''，以节省存储空间。

6. 建议使用NUMERIC，而不用real, double precision来存储要求精确计算的数值。

7. 建议使用NUMERIC来存储金额相关的类型，在以分为单位的情况下也可以使用integer或者bigint

8. 建议使用hstore来存储非结构化，key-value键值型。

9. 建议使用ltree来存储树状层次结构数据。

10. 建议使用json来存储JSON(JavaScript Object Notation)数据。

11. 建议使用Geometric Types结合PostGIS来实现地理信息数据存储及操作。

12. 建议对数据表字段加上COMMENT，便于后续的维护, COMMENT请使用英文。


### 1.4 数据索引
 
1. 索引按照idx_表名_字段名的规则命名。

2. 索引名长度限制在30字符以内。当长度超过30位时，可使用缩写来缩减长度。

3. 建议不要建过多的索引，一般不超过6个。核心数据表可以适当增加索引个数。  



## 2 操作规范

1. 开发测试以及相关业务操作，不要使用数据库超级用户，非常危险。

2. 避免使用[SELECT *]，只取需要的字段来减少网络带宽消耗。

3. 不要DELETE全表，请使用TRUNCATE代替。

4. 建议使用LIMIT(1)而不是COUNT(*)来判断是否有数据。

5. 避免频繁的创建和删除临时表，以减少系统表资源的消耗。

6. 数据表中定义的数据类型和程序中的定义保持一致，避免报错或者无法使用索引的情况发生。

7. 建议复杂的统计查询可以尝试[窗口函数Window Functions](https://www.postgresql.org/docs/current/static/tutorial-window.html)。

8. 建议将单个事务中的多条表操作进行拆分或者不放在单个事务中，以缩小单个事务的粒度，减少LOCK资源，避免锁等待以及死锁的发生。

9. 数据订正时，删除和修改记录时，要先SELECT，避免出现误删除，确认无误才能提交执行。

10. 建议发给DBA执行的SQL，均需要去除注释等冗余信息。  


## 3 备注

1. 填充因子示例：CREATE TABLE XXX (...) WITH (fillfactor=85);

2. 



