### Primary Key(PK)
* 不能为NULL

### Unique Key
* 可以为NULL，且可以多行为NULL（因为 NULL 表示 "unknown" ，大部分 DBMS 认为 NULL != NULL ，在 MySQL `SELECT if(NULL = NULL, 1, 0); # 0 SELECT if(NULL != NULL, 1, 0); # 0` ）

### Key
* 其实就是个普通索引

### 修改Key
```
ALTER TABLE t_cap_daily_swift_recharge_amount DROP KEY i_cap_daily_swift_recharge_amount_day, ADD UNIQUE KEY `DAY` (`DAY`);
```


## References
1. [MySql - Meaning of “PRIMARY KEY”, “UNIQUE KEY” and “KEY” when used together][1]
2. [What is the difference b/w Primary Key and Unique Key][2]
3. [12.4 Control Flow Functions][3]
4. [Change unique key together in mysql][4]

[1]: http://stackoverflow.com/questions/10908561/mysql-meaning-of-primary-key-unique-key-and-key-when-used-together "MySql - Meaning of “PRIMARY KEY”, “UNIQUE KEY” and “KEY” when used together"
[2]: http://stackoverflow.com/questions/2973420/what-is-the-difference-b-w-primary-key-and-unique-key "What is the difference b/w Primary Key and Unique Key"
[3]: http://dev.mysql.com/doc/refman/5.7/en/control-flow-functions.html "12.4 Control Flow Functions"
[4]: http://stackoverflow.com/questions/20116140/change-unique-key-together-in-mysql "Change unique key together in mysql"
