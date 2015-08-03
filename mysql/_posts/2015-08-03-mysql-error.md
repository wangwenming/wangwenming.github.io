### ERROR 1136 (21S01) at line 1: Column count doesn't match value count at row 1

*时间*: 2015-08-03

*原因*: INSERT 操作时未指定 col_name 列表，即对所有字段都传入值，如果此时字段定义有更新(如顺序变化，增加或删除了字段)，就会列匹配失败。

*方案*: 建议 INSERT 一律列出字段名，向后兼容性超好。

*举例*:

`INSERT INTO t_cap_redpackinfo VALUES (NULL,NULL,220804,0,0,NULL,'2015-07-17 15:09:12',NULL,'2015-07-17 15:09:12',NULL,0,30.00,2,278312,NULL,NULL,NULL);`

改成

`INSERT INTO t_cap_redpackinfo (USER_ID,CARI_ENABLE,CARI_STATUS,CREATE_TIME,MODIFY_TIME,STATUS,CARI_AMOUNT,CARI_TYPE,INVITEE_ID) VALUES ($USER_ID,0,0,'$CREATE_TIME','$CREATE_TIME',0,30.00,2,$INVITEE_ID), ($USER_ID,0,0,'$CREATE_TIME','$CREATE_TIME',0,100.00,5,$INVITEE_ID)`

后面新增了2个字段，虽然不用，但如果不补上2个NULL，就会报这个错。


### ERROR 1267 (HY000): Illegal mix of collations (utf8_unicode_ci,IMPLICIT) and (utf8_general_ci,IMPLICIT) for operation '='

*时间*: 2015-08-03

*原因*: 其中一个表显示的使用了 `utf8_unicode_ci` collate ， 而默认的则是 `utf8_general_ci` 。此时联表查询，或者子查询都会报这个错误。

*方案*:

`ALTER TABLE t_wx_subscribe CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;`

重置表的 COLLATE 的同时，自动去除字段定义中的 COLLATE 。

*备注*: 用 `CHANGE COLUMN` 去掉字段 `OPEN_ID varchar(255) COLLATE utf8_unicode_ci NOT NULL` 里的 `COLLATE` 没有效果，反而是上面的方案会自动去掉。




