超时:
t_yeepay_reconciliation._2015-07-18.part4.csv

Warnings
csv/t_yeepay_reconciliation._2015-07-18.part16.csv 70
csv/t_yeepay_reconciliation._2015-07-18.part21.csv 418
csv/t_yeepay_reconciliation._2015-07-18.part22.csv 813
csv/t_yeepay_reconciliation._2015-07-18.part23.csv 114

原因: AMOUNT 太长了。。。。
 1699 NULL,recharge,32470,孙瑞琦,NULL,NULL,15967849877,B20150625000012947348,bc8f0b2ceadf4a139eef954f10039e26,2015-06-25 00:00:14,2015-06-25       00:01:22,91.1999969482422,NULL,NULL,NULL,NULL,NULL,NULL,NULL,0

 显示是 91.20

 B20150625000050841357 91.0299987792969 => 91.03


Note    1265    Data truncated for column 'AMOUNT' at row 1


B20150625083756985469,0.00999999977648258  => 0.01
B20150625012848602886,0.0299999993294477   => 0.03
B20150625093306505233,91.0299987792969     => 91.03
B20150625093628209284,91.0999984741211     => 91.10



cut -d"," -f8,12  csv/t_yeepay_reconciliation._2015-07-18.part21.csv | awk -F"," '{if (match($2,/[0-9]+\.[0-9]{3,}/)) print}'

https://www.gnu.org/s/gawk/manual/html_node/Round-Function.html

awk
$ echo "1.5" | awk '{printf("%.f\n", $1)}'
2
$ echo "2.5" | awk '{printf("%.f\n", $1)}'
2
