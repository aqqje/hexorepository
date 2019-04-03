---
title: 'mysql-error:1366-incorrect string value:xxxx'
date: 2019-04-03 23:51:02
tags: mysql
---

#### 问题:mysql-error:1366-incorrect string value:xxxx?

##### 1.解决:

 - ALTER TABLE user CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;

##### 2.拓展

```sql
-- 检查当前表中字段的字符集设置
show full fields from user;

-- 检查当前表中字段的字符集设置
show create table table;

-- 修改数据库的默认字符集
ALTER DATABASE user DEFAULT CHARACTER SET  utf8 COLLATE utf8_general_ci;

-- 修改表的默认字符集
ALTER TABLE user DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

-- 修改表和所有字符列（CHAR,VARCHAR,TEXT）默认的字符集
ALTER TABLE user CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;

-- 修改字段的字符集
ALTER TABLE user CHANGE title title VARCHAR(100) CHARACTER SET utf8 COLLATE utf8_general_ci;

-- 查看数据库编码
SHOW CREATE DATABASE dome;

-- 查看表编码
SHOW CREATE TABLE user;

-- 查看字段编码
SHOW FULL COLUMNS FROM user;

```

##### 参考:

[在一个字段输入中文是出现错误1366-incorrect string value:\xE7\x8E\x8B for column fi](https://blog.csdn.net/weixin_40539892/article/details/80227981)