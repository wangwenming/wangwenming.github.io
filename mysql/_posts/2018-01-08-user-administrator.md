### 新建用户
```mysql
> CREATE USER ec IDENTIFIED BY 'ec';
```

### 修改密码
```
> SET PASSWORD FOR ec = PASSWORD('ec');
```

### 查看用户权限
SHOW GRANTS FOR ec;

```
mysql> SHOW GRANTS FOR ec;
+---------------------------------------------------------------------------------------------------+
| Grants for ec@%                                                                                   |
+---------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'ec'@'%' IDENTIFIED BY PASSWORD '*C5FE07279CCA84292104D1504B130C63A73E07D0' |
+---------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)
```

### 赋予权限
```
> GRANT SELECT ON ec_thb.* TO ec;
```

```
> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE ON ec_ceb.* TO ec;
```

### 回收权限
```
> REVOKE SELECT ON ec_thb.* from ec;
```

### 权限补充说明
GRANT 和 REVOKE 可以在几个层次上控制访问权限
1. 整个服务器，使用 GRANT ALL 和 REVOKE ALL
2. 整个数据库，使用 ON database.*
3. 特定表，使用 ON database.table
4. 特定的列
5. 特定的存储过程

```
# 某个数据库的权限
> GRANT ALL PRIVILEGES ON testdb.* TO dba@'localhost'
# 所有数据库的权限
> GRANT ALL ON *.* TO dba@'localhost'
```

### 刷新权限
```
> FLUSH PRIVILEGES;
```

### user 表中 Host 列的值的意义

列|含义
--|----
% | 匹配所有主机
localhost | localhost不会被解析成IP地址，直接通过 *UNIXsocket* 连接
127.0.0.1 | 会通过TCP/IP协议连接，并且只能在本机访问
::1 | ::1就是兼容支持ipv6的，表示同ipv4的127.0.0.1

