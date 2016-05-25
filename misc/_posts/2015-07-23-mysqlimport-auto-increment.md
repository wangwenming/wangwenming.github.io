Bug of mysqlimport

mysql> SELECT COUNT(1),MIN(ID),MAX(ID) FROM t_yeepay_reconciliation;
+----------+---------+---------+
| COUNT(1) | MIN(ID) | MAX(ID) |
+----------+---------+---------+
|   239085 |       1 |  385894 |
+----------+---------+---------+
1 row in set (0.39 sec)


mysql> SELECT min(id) FROM t_yeepay_reconciliation where id > 10000;
+---------+
| min(id) |
+---------+
|   16384 |
+---------+
1 row in set (0.00 sec)

Math.pow(2,14);
16384



mysql> SELECT max(id),min(id) FROM t_yeepay_reconciliation where id>20000;
+---------+---------+
| max(id) | min(id) |
+---------+---------+
|   52767 |   32768 |
+---------+---------+
1 row in set (0.01 sec)


http://www.ggask.com/139d8aa.htm

过程是这样：
插入 x 条数据前，先把 AUTO_INCREMENT + 1个>x的2的N次方


TODO: 如果 LOCK table 是不是就没问难题。
https://dev.mysql.com/doc/refman/5.0/en/lock-tables.html



