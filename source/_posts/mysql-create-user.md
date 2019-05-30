---
title: MySQL 创建用户，并赋予权限
tags:
  - MySQL
  - 创建用户
  - 权限
date: 2019-05-30 16:14:09
---

```sql
-- 创建用户
CREATE USER '用户名'@'访问主机' IDENTIFIED BY '密码';

-- 赋予权限
GRANT 权限列表 ON 数据库 TO '用户名'@'访问主机' ;

-- 赋予所有数据库的权限
GRANT ALL PRIVILEGES ON *.* TO '用户名'@'访问主机' ;

-- 修改权限
GRANT 权限列表 on 数据库 to '用户名'@'访问主机' WITH GRANT OPTION;
```
