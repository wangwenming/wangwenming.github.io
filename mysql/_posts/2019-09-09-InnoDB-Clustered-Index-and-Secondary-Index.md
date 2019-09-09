# InnoDB Clustered Index and Secondary Index

## Clustered Index

MySQL InnoDB 中的索引分为 ```Clustered Index``` (聚簇索引)和 ```Secondary Index``` (二级索引)

每一个 InnoDB 表都有一个特殊的索引，叫做 ```Clustered Index```，通常来讲，```Clustered Index``` 和 ```Primary Key``` 是同一个意思，InnoDB 选择```Clustered Index``` 原则如下：

1. 如果表上定义了 ```Primary Key``` ，则使用 ```Primary Key``` 作为 ```Clustered Index```
2. 如果没有定义 ```Primary Key``` ，选择第一个非空的 UNIQUE 索引作为 ```Clustered Index``` 。
3. 如果即没有 ```Primary Key``` ，也没有合适的 UNIQUE 索引，InnoDB 内部产生一个隐藏列，这个列包含了每一行的 ```row ID``` ， ```row ID```随着新行的插入而单调增加。然后在这个隐藏列上建立索引作为 ```Clustered Index``` 。


## Secondary Index

除了 ```Clustered Index``` 之外的索引都是 ```Secondary Index``` ，每一个 ```Secondary Index```  的记录中除了索引列的值之外，还包含主健值。通过二级索引查询首先查到是主键值，然后 InnoDB 再根据查到的主键值通过主键/聚簇索引找到相应的数据块。

> Accessing a row through the clustered index is fast because the index search leads directly to the page with all the row data. 

> If the primary key is long, the secondary indexes use more space, so it is advantageous to have a short primary key.

## References

2014-04-29 [MySQL存储引擎中的MyISAM和InnoDB](https://www.biaodianfu.com/mysql-myisam-innodb.html)