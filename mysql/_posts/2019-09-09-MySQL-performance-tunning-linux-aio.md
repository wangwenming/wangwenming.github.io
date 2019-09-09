## 如何查看 ***innodb*** 表所占磁盘大小

```
SELECT 
    table_name AS `Table`, 
    ROUND(((data_length + index_length) / 1024 / 1024), 2) `表总体大小(MB)`,
    ROUND((data_length / 1024 / 1024), 2) `数据大小(MB)`,
    ROUND((index_length / 1024 / 1024), 2) `索引大小(MB)`
FROM information_schema.TABLES 
WHERE table_schema = "huizhaofang"
    AND table_name = "subpay";
```

## 如何统计进程的系统调用(sys call)
```
$ sudo strace -fcp 22517
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 63.29   15.949796        1769      9017           io_getevents
 20.62    5.196000       34872       149        51 futex
 12.02    3.028000      151400        20           select
  4.05    1.020000      170000         6         5 restart_syscall
  0.02    0.006019           0     45304           pread
  0.00    0.001129           0      9923           io_submit
  0.00    0.000000           0        64           sched_yield
  0.00    0.000000           0         1           sendto
  0.00    0.000000           0         3         1 recvfrom
------ ----------- ----------- --------- --------- ----------------
100.00   25.200944                 64487        57 total
```

### 浅析Linux异步IO(AIO)：```io_submit/io_setup/io_getevents```

1.gcc AIO

> gcc遵循posix标准实现了AIO。头文件为```<aio.h>```，支持FreeBSD/Linux。是通过阻塞IO+线程池来实现的。主要的几个函数是```aio_read```/```aio_write```/```aio_return```。

```io_prep_pread``` 也是这里的函数。

> 优点：支持平台多，兼容性好，无需依赖第三方库，阻塞IO可以利用到操作系统的PageCache。

> 缺点：据说有一些bug和陷阱，一直未解决。不过这个都是网上文章中讲的，gcc发展这么多年，不至于还有遗留bug吧。这里有待测试。

2. Linux Native Aio

> 由操作系统内核提供的AIO，头文件为```<linux/aio_abi.h>```。Native Aio是真正的AIO，完全非阻塞异步的，而不是用阻塞IO和线程池模拟。主要的几个系统调用为```io_submit```/```io_setup```/```io_getevents```。

> 优点：由操作系统提供，读写操作可以直接投递到硬件，不会浪费CPU。
>
> 缺点：仅支持Linux，必须使用DirectIO，所以无法利用到操作系统的PageCache。对于写文件来说native aio的作用不大，因为本身写文件就是先写到PageCache上，直接返回，没有IO等待。

3. Libeio

> libev的作者开发的AIO实现，与gcc aio类似也是使用阻塞IO+线程池实现的。优点与缺点参见上面。它与gcc aio的不同之处，代码更简洁，所以bug少更安全稳定。但这是一个第三方库，你的代码需要依赖libeio。

```
$ apt-cache search libaio
libaio-dev - Linux kernel AIO access library - development files
$ sudo apt-get install libaio-dev
$ gcc -o aio.o aio.c -laio
$ ./aio

# 可以看到打开了 /etc/passwd 文件
$ sudo lsof -p 
# 统计系统调用
$ sudo strace -cfp  9425
```

## Linux ABI(Application Binary Interface) 应用程序二进制接口

[Application binary interface - Wikipedia](https://en.wikipedia.org/wiki/Application_binary_interface):
> Linux kernel and GNU C Library define the Linux API. After compilation, the binaries offer an ABI
> 
> 

### 
```
mysql> SHOW VARIABLES LIKE 'innodb_use_native_aio%';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_use_native_aio | ON    |
+-----------------------+-------+
1 row in set (0.01 sec)
```

> 听起来Kernel Native AIO 几乎提供了近乎完美的异步方式，但如果你对它抱有太高期望的话，你会再一次感到失望.

> 目前的Kernel AIO 仅支持 O_DIRECT 方式来对磁盘读写，这意味着，你无法利用系统的缓存，同时它要求读写的的大小和偏移要以区块的方式对齐,参考nginx 的作者 Igor Sysoev 的评论： http://forum.nginx.org/read.php?2,113524,113587#msg-113587



## References
* [82 Posix Headers](https://pubs.opengroup.org/onlinepubs/9699919799/idx/head.html)
* [linux下的异步IO（AIO）是否已成熟？](https://www.zhihu.com/question/26943558)
* [InnoDB异步IO(AIO)实现详解](http://hedengcheng.com/?p=98)
* [15.8.6 Using Asynchronous I/O on Linux](https://dev.mysql.com/doc/refman/8.0/en/innodb-linux-native-aio.html)
* [linux AIO （异步IO） 那点事儿 [转]](https://yq.aliyun.com/articles/341711)
