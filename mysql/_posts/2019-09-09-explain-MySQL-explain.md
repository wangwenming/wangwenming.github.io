## EXPLAIN

* ***type***

1.***type***=***const***

> The table has at most one matching row,

```
EXPLAIN SELECT * FROM subrent WHERE subrent_id = 3;
```
***备注*** 要返回一条记录

2.***type***=***eq_ref***

> One row is read from this table for each combination of rows from the previous tables. 

左边表一行，对应当前表一行。

3. ***type***=***ref***

> All rows with matching index values are read from this table for each combination of rows from the previous tables

> If the key that is used matches only a few rows, this is a good join type.

左边表一行，对应当前表多行。

4. ***type***=***ref_or_null***

> This join type is like ref, but with the addition that MySQL does an extra search for rows that contain NULL values.
> 

4. ***type***=***all***

> A full table scan is done for each combination of rows from the previous tables. This is normally ***not good*** if the table is the first table not marked const, and usually ***very bad*** in all other cases. 

例如：

```SQL
SELECT * FROM subrent LIMIT 1;
```

遇到这种通常要格外注意优化，不过上面这个 SQL 并不慢。

5. ***type*** = ***index***

> The index join type is the same as ALL, except that the index tree is scanned.

如果只扫描 index tree 就能完成，则 Extra 列显示 ```Using index``` ，如果还要进行 full table scan 则不显示。

> MySQL can use this join type when the query uses only columns that are part of a single index.

### Extra

1. Using index

> The column information is retrieved from the table using only information in the index tree without having to do an additional seek to read the actual row.

只需要用到 index tree 不用全表扫描。

2. Using where

> A WHERE clause is used to restrict which rows to match against the next table or send to the client. Unless you specifically intend to fetch or examine all rows from the table, you may have something wrong in your query if the Extra value is not Using where and the table join type is ALL or index.

## 优化实战

```
SET GLOBAL innodb_buffer_pool_load_now='ON';

show global status like 'innodb_buffer_pool%';
```

```
SELECT concat('`',cs.contract_no) AS contract_no ,cs.agency_id AS agency_id ,cs.agency_name AS agency_name ,sp.subpayid as boss_subpay_id ,CASE WHEN cs.contract_status = 3 and date(cs.breach_time) <= '2019-07-31' THEN '违约' WHEN cs.contract_status = 2 and (select date(IFNULL(pay_time, IFNULL(offline_pay_time, IFNULL(remark_pay_time, pay_date)))) from subpay where contract_no=cs.contract_no and status in (2,5) order by `index` desc limit 1 ) <= '2019-07-31' THEN '结束' WHEN cs.contract_status in (1,2,3) THEN '正常' ELSE cs.contract_status END AS contract_status ,date(IF(cs.contract_status = 3 and date(cs.breach_time) <= '2019-07-31', cs.breach_time, '')) as boss_refund_time ,sp.`index` AS subpay_index ,sp.sources_capital AS sources_capital ,IF(sr.status = 2,'已出纳','未出纳') AS is_cashing ,TIMESTAMP(IF(date(IFNULL(sr.cashing_time, sr.pay_date)) <= '2019-07-31', IFNULL(sr.cashing_time, sr.pay_date), ''))as cashing_time ,date(sp.pay_date) AS boss_pay_date ,date(IFNULL(sp.pay_time,IF(sp.status in( 2,5) AND DATE(IFNULL(IFNULL(IFNULL(sp.pay_time, sp.offline_pay_time),sp.remark_pay_time),sp.pay_date)) <= '2019-07-31',sp.remark_pay_time, ''))) AS boss_flat_time ,IF(sp.status IN (2,5) AND DATE(IFNULL(IFNULL(IFNULL(sp.pay_time, sp.offline_pay_time),sp.remark_pay_time),sp.pay_date)) <='2019-07-31', IFNULL((SELECT channel FROM db_charge_platform.charge WHERE order_no=sp.success_order_no and status=2 LIMIT 1), '线下'), '') AS charge_type ,(SELECT concat('`',cg.platform_order_id) from db_charge_platform.charge cg where cg.order_no=sp.success_order_no and cg.state = 1 and cg.status = 2 ) boss_success_order_no ,if(sp.status IN (2,5) AND DATE(IFNULL(IFNULL(IFNULL(sp.pay_time, sp.offline_pay_time),sp.remark_pay_time),sp.pay_date)) <='2019-07-31', CONVERT((sp.price / 100), DECIMAL(65,2)), NULL) AS boss_price ,IF(sp.status in (2,5) AND DATE(IFNULL(IFNULL(IFNULL(sp.pay_time, sp.offline_pay_time), sp.remark_pay_time), sp.pay_date)) <= '2019-07-31', CONVERT((sp.overdue_fine / 100), DECIMAL(65,2)), 0) AS boss_overdue_fine ,CONVERT((sp.real_base_price / 100), DECIMAL(65,2)) AS boss_real_base_price ,CONVERT((sp.real_service_fee / 100), DECIMAL(65,2)) AS boss_real_service_fee , sp.status as boss_status FROM subpay AS sp LEFT join subrent AS sr on sp.contract_no=sr.contract_no and sr.index = sp.subrent_index LEFT JOIN contract_snapshot AS cs on sp.contract_no=cs.contract_no WHERE cs.state = 1 AND sp.state = 1 and sr.state=1 and sr.status in (2,5) AND cs.contract_status!=6 AND cs.lease_type !=5 AND date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-31' AND sr.first_sources_capital = 'h028' AND sr.end_sources_capital = 'h028';
```

### 实战1
```
EXPLAIN SELECT SQL_NO_CACHE
  COUNT(1)
FROM 
  subpay sp 
LEFT JOIN subrent AS sr ON
  sp.contract_no = sr.contract_no AND sr.INDEX = sp.subrent_index
LEFT JOIN contract_snapshot cs ON
  sp.contract_no = cs.contract_no 
WHERE 
  cs.state = 1 AND
  sp.state = 1 AND
  sr.state = 1 AND
  sr.status IN (2, 5) AND
  cs.contract_status != 6 AND 
  cs.lease_type !=5 AND
  date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25' AND
  sr.first_sources_capital = 'h028' AND
  sr.end_sources_capital = 'h028';
```

返回 ***220300***行，其中3个表的行数为：

1. subpay: 6886505 
2. subrent: 934740
3. contract_snapshot: 716574

```
+----+-------------+-------+------+---------------------------------+-----------------+---------+----------------------------+--------+------------------------------------+
| id | select_type | table | type | possible_keys                   | key             | key_len | ref                        | rows   | Extra                              |
+----+-------------+-------+------+---------------------------------+-----------------+---------+----------------------------+--------+------------------------------------+
|  1 | SIMPLE      | cs    | ALL  | contract_no,idx_contract_status | NULL            | NULL    | NULL                       | 670989 | Using where                        |
|  1 | SIMPLE      | sr    | ref  | idx_contract_no                 | idx_contract_no | 99      | huizhaofang.cs.contract_no |      1 | Using index condition; Using where |
|  1 | SIMPLE      | sp    | ref  | idx_contract_no                 | idx_contract_no | 99      | huizhaofang.cs.contract_no |      4 | Using index condition; Using where |
+----+-------------+-------+------+---------------------------------+-----------------+---------+----------------------------+--------+------------------------------------+
```

时间：
[7.77, 9.62, 7.88, 8.21, 7.71, 7.49, 8.20] = 7.954

#### 优化1：最有效的字段加索引
```
ALTER TABLE subrent ADD INDEX idx_end_sources_capital (end_sources_capital);
Query OK, 0 rows affected (2.84 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

```
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
| id | select_type | table | type   | possible_keys                           | key                     | key_len | ref                        | rows  | Extra                              |
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
|  1 | SIMPLE      | sr    | ref    | idx_contract_no,idx_end_sources_capital | idx_end_sources_capital | 18      | const                      | 67556 | Using index condition; Using where |
|  1 | SIMPLE      | sp    | ref    | idx_contract_no                         | idx_contract_no         | 99      | huizhaofang.sr.contract_no |     4 | Using where                        |
|  1 | SIMPLE      | cs    | eq_ref | contract_no,idx_contract_status         | contract_no             | 302     | huizhaofang.sr.contract_no |     1 | Using index condition; Using where |
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
```

[4.15, 4.25, 4.07, 4.05, 3.96, 4.00, 4.17] = 4.088

#### 优化2：不要索引 varchar 改为 char
```
mysql> ALTER TABLE subrent MODIFY `end_sources_capital` CHAR(4) NOT NULL DEFAULT '' COMMENT '最后资金来源';                                                                                                                                                                     Query OK, 934740 rows affected (28.04 sec)
Records: 934740  Duplicates: 0  Warnings: 0
```

```
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
| id | select_type | table | type   | possible_keys                           | key                     | key_len | ref                        | rows  | Extra                              |
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
|  1 | SIMPLE      | sr    | ref    | idx_contract_no,idx_end_sources_capital | idx_end_sources_capital | 12      | const                      | 66752 | Using index condition; Using where |
|  1 | SIMPLE      | sp    | ref    | idx_contract_no                         | idx_contract_no         | 99      | huizhaofang.sr.contract_no |     4 | Using where                        |
|  1 | SIMPLE      | cs    | eq_ref | contract_no,idx_contract_status         | contract_no             | 302     | huizhaofang.sr.contract_no |     1 | Using index condition; Using where |
+----+-------------+-------+--------+-----------------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
```
[3.42, 3.86, 4.71, 3.96, 3.78, 4.16, 3.78] 3.908

#### 优化3：联表务必只有主键（1）
```
mysql> ALTER TABLE subpay ADD COLUMN subrent_id INT(11) NOT NULL DEFAULT 0 COMMENT "对应的打款单ID FOREIGN KEY REFERENCES subrent(subrent_id)" AFTER subpayid;
Query OK, 0 rows affected (6 min 39.42 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> UPDATE subpay sp JOIN subrent sr ON sp.contract_no = sr.contract_no AND sr.`index` = sp.subrent_index SET sp.subrent_id = sr.subrent_id;
Query OK, 6882251 rows affected (2 min 27.96 sec)
Rows matched: 6882251  Changed: 6882251  Warnings: 0

mysql> ALTER TABLE subpay ADD INDEX idx_subrent_id (subrent_id);
Query OK, 0 rows affected (20.81 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

对应的查询语句修改为：

```
EXPLAIN SELECT SQL_NO_CACHE
  COUNT(1)
FROM 
  subpay sp 
LEFT JOIN subrent sr USING(subrent_id)
LEFT JOIN contract_snapshot cs ON
  sp.contract_no = cs.contract_no 
WHERE 
  cs.state = 1 AND
  sp.state = 1 AND
  sr.state = 1 AND
  sr.status IN (2, 5) AND
  cs.contract_status != 6 AND 
  cs.lease_type !=5 AND
  date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25' AND
  sr.first_sources_capital = 'h028' AND
  sr.end_sources_capital = 'h028';
```

```
+----+-------------+-------+--------+---------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
| id | select_type | table | type   | possible_keys                   | key                     | key_len | ref                        | rows  | Extra                              |
+----+-------------+-------+--------+---------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
|  1 | SIMPLE      | sr    | ref    | PRIMARY,idx_end_sources_capital | idx_end_sources_capital | 12      | const                      | 66752 | Using index condition; Using where |
|  1 | SIMPLE      | sp    | ref    | idx_contract_no,idx_subrent_id  | idx_subrent_id          | 4       | huizhaofang.sr.subrent_id  |     3 | Using where                        |
|  1 | SIMPLE      | cs    | eq_ref | contract_no,idx_contract_status | contract_no             | 302     | huizhaofang.sp.contract_no |     1 | Using index condition; Using where |
+----+-------------+-------+--------+---------------------------------+-------------------------+---------+----------------------------+-------+------------------------------------+
```
[3.12, 2.74, 2.79, 3.04, 3.29, 2.97, 2.97] = 3.078

#### 优化4：联表务必只有主键（2）

```
EXPLAIN SELECT SQL_NO_CACHE
  COUNT(1)
FROM 
  subpay sp 
LEFT JOIN subrent sr USING(subrent_id)
LEFT JOIN contract_snapshot cs ON
  sr.contract_snapshot_id = cs.snapshot_id
WHERE 
  cs.state = 1 AND
  sp.state = 1 AND
  sr.state = 1 AND
  sr.status IN (2, 5) AND
  cs.contract_status != 6 AND 
  cs.lease_type !=5 AND
  date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25' AND
  sr.first_sources_capital = 'h028' AND
  sr.end_sources_capital = 'h028';
```

```
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
| id | select_type | table | type   | possible_keys                                    | key                     | key_len | ref                                 | rows  | Extra                              |
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
|  1 | SIMPLE      | sr    | ref    | PRIMARY,subrent_423032bd,idx_end_sources_capital | idx_end_sources_capital | 12      | const                               | 66752 | Using index condition; Using where |
|  1 | SIMPLE      | cs    | eq_ref | PRIMARY,idx_contract_status                      | PRIMARY                 | 4       | huizhaofang.sr.contract_snapshot_id |     1 | Using where                        |
|  1 | SIMPLE      | sp    | ref    | idx_subrent_id                                   | idx_subrent_id          | 4       | huizhaofang.sr.subrent_id           |     3 | Using where                        |
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
```

[3.02, 2.08, 2.30, 2.25, 2.21, 2.27, 2.41] = 2.288

#### 优化5：删除 sp.state = 1 条件

```
EXPLAIN SELECT SQL_NO_CACHE
  COUNT(1)
FROM 
  subpay sp 
LEFT JOIN subrent sr USING(subrent_id)
LEFT JOIN contract_snapshot cs ON
  sr.contract_snapshot_id = cs.snapshot_id
WHERE 
  cs.state = 1 AND
  sr.state = 1 AND
  sr.status IN (2, 5) AND
  cs.contract_status != 6 AND 
  cs.lease_type !=5 AND
  date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25' AND
  sr.first_sources_capital = 'h028' AND
  sr.end_sources_capital = 'h028';
```

subpay表的 Using where 变成 Using index

```
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
| id | select_type | table | type   | possible_keys                                    | key                     | key_len | ref                                 | rows  | Extra                              |
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
|  1 | SIMPLE      | sr    | ref    | PRIMARY,subrent_423032bd,idx_end_sources_capital | idx_end_sources_capital | 12      | const                               | 66752 | Using index condition; Using where |
|  1 | SIMPLE      | cs    | eq_ref | PRIMARY,idx_contract_status                      | PRIMARY                 | 4       | huizhaofang.sr.contract_snapshot_id |     1 | Using where                        |
|  1 | SIMPLE      | sp    | ref    | idx_subrent_id                                   | idx_subrent_id          | 4       | huizhaofang.sr.subrent_id           |     3 | Using index                        |
+----+-------------+-------+--------+--------------------------------------------------+-------------------------+---------+-------------------------------------+-------+------------------------------------+
```

[0.77, 0.79, 0.83, 0.81, 0.83, 0.77, 0.87] = 0.806

#### 最终 SQL
```
EXPLAIN SELECT SQL_NO_CACHE
  Concat('`',cs.contract_no) AS contract_no,
  cs.agency_id AS agency_id, 
  cs.agency_name AS agency_name, 
  sp.subpayid AS boss_subpay_id, 
  CASE 
    WHEN cs.contract_status = 3 AND Date(cs.breach_time) <= '2019-07-25' THEN '违约' 
    WHEN cs.contract_status = 2 AND ( 
      SELECT Date(Ifnull(pay_time, Ifnull(offline_pay_time, Ifnull(remark_pay_time, pay_date))))
        FROM subpay 
        WHERE contract_no = cs.contract_no 
          AND status IN (2,5) 
        ORDER BY `index` DESC limit 1 ) <= '2019-07-25' THEN '结束' 
    WHEN cs.contract_status IN (1,2,3) THEN '正常' 
    ELSE cs.contract_status 
  END AS contract_status, 
  date(IF(cs.contract_status = 3 AND Date(cs.breach_time) <= '2019-07-25', cs.breach_time, '')) AS boss_refund_time, 
  sp.`INDEX` AS subpay_index, 
  sp.sources_capital AS sources_capital, 
  IF(sr.status = 2, '已出纳', '未出纳') AS is_cashing,
  
  timestamp(IF(date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25', ifnull(sr.cashing_time, sr.pay_date), '')) AS cashing_time,
  
  date(sp.pay_date) AS boss_pay_date,

  date(ifnull(sp.pay_time, IF(sp.status IN(2, 5) AND date(ifnull(ifnull(ifnull(sp.pay_time, sp.offline_pay_time),sp.remark_pay_time),sp.pay_date)) <= '2019-07-25', sp.remark_pay_time, ''))) AS boss_flat_time, 
  

  IF(sp.status IN (2, 5) AND 
date(ifnull(ifnull(ifnull(sp.pay_time, sp.offline_pay_time),sp.remark_pay_time),sp.pay_date)) <='2019-07-25', CONVERT((sp.price / 100), decimal(65,2)), NULL) AS boss_price , 

  IF(sp.status IN (2, 5) AND 
date(ifnull(ifnull(ifnull(sp.pay_time, sp.offline_pay_time), sp.remark_pay_time), sp.pay_date)) <= '2019-07-25', CONVERT((sp.overdue_fine / 100), decimal(65,2)), 0) AS boss_overdue_fine,

  CONVERT((sp.real_base_price / 100), decimal(65,2)) AS boss_real_base_price,

  CONVERT((sp.real_service_fee / 100), decimal(65,2)) AS boss_real_service_fee,

  sp.status AS boss_status 
FROM 
  subpay sp 
LEFT JOIN subrent sr USING(subrent_id)
LEFT JOIN contract_snapshot cs ON
  sr.contract_snapshot_id = cs.snapshot_id
WHERE 
  cs.state = 1 AND
  sr.state = 1 AND
  sr.status IN (2, 5) AND
  cs.contract_status != 6 AND 
  cs.lease_type !=5 AND
  date(ifnull(sr.cashing_time, sr.pay_date)) <= '2019-07-25' AND
  sr.first_sources_capital = 'h028' AND
  sr.end_sources_capital = 'h028'
  LIMIT 20;
```


```
EXPLAIN SELECT SQL_NO_CACHE
  COUNT(1)
FROM 
  subpay sp 
LEFT JOIN subrent sr USING(subrent_id)
LEFT JOIN contract_snapshot cs ON
  sr.contract_snapshot_id = cs.snapshot_id
WHERE 
  sr.end_sources_capital = 'h028';
```

## References
* 2015-02-19 [MySQL Dumping and Reloading the InnoDB Buffer Pool](https://mysqlserverteam.com/mysql-dumping-and-reloading-the-innodb-buffer-pool/)
* [8.8.2 EXPLAIN Output Format](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html)