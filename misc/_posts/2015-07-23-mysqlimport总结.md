mysqlimport总结

mysqlimport --fields-optionally-enclosed-by='"' --fields-terminated-by="," --local --low-priority --debug --debug-info --verbose --host=180.150.189.229 --port 16034 --user dwlc_test --password=dwlctest dwlc_test t_yeepay_reconciliation.csv



--fields-optionally-enclosed-by='"' 会兼容两边有 " 的情况，否则报错，字段值为 NULL
如果没有值（csv 文件： ,,），则使用默认值，并报一个 warning， 如 FEE 默认是 0.00000



