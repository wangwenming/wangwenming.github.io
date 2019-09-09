## 磁盘IO

磁盘读取有两种模式：
* Sequential I/O(顺序I/O)
* Random I/O(随机I/O)

磁盘读取操作步骤：
1. seek time(寻址时间): 

***Sending data***
> The thread is reading and processing rows for a SELECT statement, and sending data to the client. Because operations occurring during this state tend to perform large amounts of disk access (reads), it is often the longest-running state over the lifetime of a given query.
> 

### hdparm - get/set SATA/IDE device parameters

```
# -t 评估硬盘的读取效率(buffered disk reads)
# -T 评估硬盘快取的读取效率(cached reads)
# -g Display the drive geometry (cylinders柱面, heads磁头, sectors扇区), the size (in sectors) of the device, and the starting offset (in sectors) of the device from the beginning of the drive.
$ sudo hdparm -tT /dev/xvda

/dev/xvda:
 Timing cached reads:   12874 MB in  1.98 seconds = 6499.63 MB/sec
 Timing buffered disk reads: 130 MB in  3.03 seconds =  42.95 MB/sec
```


[The Seagate 600 & 600 Pro SSD Review](https://www.anandtech.com/show/6935/seagate-600-ssd-review/5) ，Samsung SSD 840 Pro 512GB (Data Transfer Rate = 6Gbps) 的 ***4KB随机读*** 速度为 ***103MB/s*** ；顺序读 421.6MB/S

STRAIGHT_JOIN

## Late row lookup 延迟行查找

> However, MySQL always does early row lookup: it searches for a row prior to checking values in the index, even the optimizer decided to use the index.


[MySQL ORDER BY / LIMIT performance: late row lookups](https://explainextended.com/2009/10/23/mysql-order-by-limit-performance-late-row-lookups/)


### 
* [13.7.6.31 SHOW PROFILES Syntax](https://dev.mysql.com/doc/refman/8.0/en/show-profiles.html)
* [8.14.2 General Thread States](https://dev.mysql.com/doc/refman/8.0/en/general-thread-states.html)
* [26.19.1 Query Profiling Using Performance Schema](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-query-profiling.html)
