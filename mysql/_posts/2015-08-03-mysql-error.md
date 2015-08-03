2015-08-03-mysql-error.md

### ERROR 1136 (21S01) at line 1: Column count doesn't match value count at row 1
*时间*: 2015-08-03
*原因*: INSERT 操作时未指定 col_name 列表，即对所有字段都传入值，如果此时字段定义有更新(如顺序变化，增加或删除了字段)，就会列匹配失败。
*方案*: 建议 INSERT 一律列出字段名，向后兼容性超好。
*举例*:
`INSERT INTO t_cap_redpackinfo VALUES (NULL,NULL,220804,0,0,NULL,'2015-07-17 15:09:12',NULL,'2015-07-17 15:09:12',NULL,0,30.00,2,278312,NULL,NULL,NULL);`
后面新增了2个字段，虽然不用，但如果不补上2个NULL，就会报这个错。
