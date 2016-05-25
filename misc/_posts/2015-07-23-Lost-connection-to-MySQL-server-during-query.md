echo ' "   aa b   "' | sed -Ee 's/(^[\s"]+)|([\s"]+$)/__/g'



# 去除双引号，然后去除前面、后面的空白符
 25             #field="${field##*( )}"
 26             #field="${field%%*( )}"
 27             #fields[$count]=$field
 28
 29             #fields[$count]=`echo $field | sed -Ee 's/(^[ "]*)|([ "]+$)//g'`


ID,TYPE,USER_ID,REAL_NAME,RELATED_USER_ID,RELATED_REAL_NAME,MOBILE,REQUEST_NO,ORDER_NO,REQUEST_TIME,DEAL_TIME,AMOUNT,FEE,ORDER_TYPE,ORDER_STATUS,REPAYMENT_AMOUNT,ROYALTY,LOAN_ID,LOAN_CREDIT_TOTAL,INTACT



insert into t_yeepay_reconciliation values (NULL,'recharge',7,'test',NULL,NULL,'13693516340','B20150304144703182012','8a5bd71c60aa479fac08b8e400a3f86e','2015-03-04 14:47:03','2015-03-04 14:49:21',1.00,0.00,'test','test',NULL,NULL,NULL,NULL,1);show warnings;


mysqlimport --default-character-set="utf8" --fields-terminated-by="," --fields-optionally-enclosed-by="'" --ignore-lines=1 --local --low-priority --debug --debug-info --verbose --host=180.150.189.229 --port 16034 --user dwlc_test --password dwlc_test t_yeepay_reconciliation.txt



mysqlimport: Error: 2013, Lost connection to MySQL server during query, when using table: t_yeepay_reconciliation
怎么解决：


PATH=`cd "$(dirname $0)" && pwd`
导致后续：
./download.sh: line 53: curl: command not found
./download.sh: line 54: xls2csv: command not found



