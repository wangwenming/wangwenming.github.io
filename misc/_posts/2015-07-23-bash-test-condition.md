10:51 - 14:29 重新导截止前天的交易记录。
14:30 - 14:49 导入 2015-07-20 的数据。Crontab: 每天凌晨5点跑前一天的数据 已验证。
15:05 - XX:XX 余额对账


# 这个是Bug，不能证明啥
$ if ! [[ true ]] && [[ false ]]; then echo "AND优先级高" ; else echo "NOT优先级高"; fi
NOT优先级高

$ if ! true && false; then echo "AND优先级高" ; else echo "NOT优先级高"; fi
NOT优先级高

$ if ! [[ 1 -eq 1 ]] && [[ 1 -gt 1 ]]; then echo "AND优先级高" ; else echo "NOT优先级高"; fi
NOT优先级高

$ if ! (true && false); then echo "true" ; else echo "false"; fi
true

$ if true ; then echo 1; else echo 0; fi
1
$ if false ; then echo 1; else echo 0; fi
0

$ if [[ true ]]; then echo 1; else echo 0; fi
1
$ if [[ false ]]; then echo 1; else echo 0; fi
1
