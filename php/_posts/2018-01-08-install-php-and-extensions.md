## 安装PHP扩展的不同方式

### Windows
通常不需要从源码编译，官方支持的扩展都已经打包好了，只需要在 *php.ini* 配置使用。

### Ubuntu
也是非常便捷，不需要重新编译PHP。
```bash
sudo apt-get install php-mysql
```

### MySQL Native Driver
*MySQLi* 在 *PHP7* 是默认关闭的。

*MySQL Native Driver* (*mysqlnd*) 是推荐的客户端，比 *MySQL Client Library* (*libmysqlclient*) 性能提升，并且功能更多。*5.4.x* 版本开始默认使用 *mysqlnd* (需要编译的使用带上参数 ```--with-mysqli``` )，*5.3.x* 开始支持 *mysqlnd* ，不过任然默认使用 *libmysqlclient* 。

