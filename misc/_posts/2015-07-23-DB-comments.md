-2 取消投标/CANCEL; 1 冻结/FREEZE; 2 完成/COMPLETE; 3 流标/BIDS; 4 撤标/WITHDRAWAL; 5 转让/TRANSFER


ALTER TABLE t_loan_loan_info CHANGE COLUMN `PRO_BILLSTATUS` `PRO_BILLSTATUS` int(11) DEFAULT NULL COMMENT '业务状态。1 已立项/START_PROJECT; 2 待发布/TO_BE_RELEASED; 3 投标中/BIDDING; 4 已满标/FULL; 5 放款中/LOANING; 6 还款中/REPAYMENT; 7 已还清/ENDED; 8 异常/EXCEPTION; 9 延期/DELAY; 10 流标/BIDS; 11 撤标/WITHDRAWAL;';


SELECT * FROM COLUMNS WHERE TABLE_NAME='t_loan_loan_info' AND COLUMN_NAME='PRO_BILLSTATUS'\G

UPDATE information_schema.COLUMNS SET COLUMN_COMMENT = 'Test' WHERE TABLE_NAME='t_loan_loan_info' AND COLUMN_NAME='PRO_BILLSTATUS';
ERROR 1044 (42000): Access denied for user 'dwlc_test'@'%' to database 'information_schema';



INFORMATION_SCHEMA provides access to database metadata.
```
They are actually views, not base tables, so there are no files associated with them, and you cannot set triggers on them.
```
INFORMATION_SCHEMA 是一个虚拟数据库，里面的 tables 其实都是 views ，而非 tables 。
https://dev.mysql.com/doc/refman/5.0/en/information-schema.html




Test数据库：
SELECT * FROM mysql.user\G
ucloudbackup
  所有权限开
root
  除如下2个权限外全开
  Shutdown_priv: N
  Create_tablespace_priv: N
dwlc_test
  Grant_priv: N


主库：
ucloudbackup
root
dwlc

$ mysql --host=180.150.189.229 --port=16034 --user=root --password
Enter password:
ERROR 1045 (28000): Access denied for user 'root'@'10.10.5.91' (using password: YES)

$ mysql --host=10.10.36.157 --user=root --password
# OK