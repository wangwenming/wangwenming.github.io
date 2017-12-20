# PHP MQTT Client

## 什么是 MQTT
MQTT(Message Queuing Telemetry Transport，消息队列遥测传输)是IBM开发的一个即时通讯协议，有可能成为物联网的重要组成部分。

此外，国内很多企业都广泛使用MQTT作为Android手机客户端与服务器端推送消息的协议。其中Sohu，Cmstop手机客户端中均有使用到MQTT作为消息推送消息。据Cmstop主要负责消息推送的高级研发工程师李文凯称，随着移动互联网的发展，MQTT由于开放源代码，耗电量小等特点，将会在移动消息推送领域会有更多的贡献，在物联网领域，传感器与服务器的通信，信息的收集，MQTT都可以作为考虑的方案之一。在未来MQTT会进入到我们生活的各各方面。

## 各种选项
### 选项1：SAM：太老，不支持PHP7
最后一次更新是 *2007-02-06* ， 版本 *1.1.0* ，支持 *PHP 4.4.2* 及以上版本。参考 [sam](http://pecl.php.net/package/sam)

实际安装会提示不支持PHP7
```bash 
$ pecl install sam
pear/SAM requires PHP (version >= 4.4.2, version <= 6.0.0), installed version is 7.1.8
No valid packages found
install failed
```

### 选项2：Mosquitto-PHP
Linux可以，Windows不支持，因为要求如下：

* PHP 5.3+
* libmosquitto 1.2.x or later
* Linux or Mac OS X. I do not have a Windows machine handy, though patches or pull requests are of course very welcome!

其优点是 API 设计较好，最近一次更新在 *2017-07-15* ，比较新。缺点就是不支持 Windows 。

【Windows】：第2条和第3条基本宣布告别Windows了。

【MAC】：可以用 *Homebrew* 安装 *Mosquitto-PHP* 
```
$ brew install mosquitto
```
安装完后， *mosquitto_sub* 和 *mosquitto_pub* 命令就有了。

【Ubuntu 14.04】：默认源只包含 libmosquitto0 (0.15-2ubuntu1.2)，版本太低。

首先添加新的源，再安装 libmosquitto-dev。参考： [Setting up MQTT Mosquitto broker in Ubuntu Linux](http://wingsquare.com/blog/setting-up-mqtt-mosquitto-broker-in-ubuntu-linux/)

```
$ sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
$ sudo apt-get update
$ # 确认一下新的源OK了
$ apt-cache search libmosquitto
...
libmosquitto1 - MQTT version 3.1/3.1.1 client library
libmosquitto-dev - MQTT version 3.1/3.1.1 client library, development files
...
$ # 按文档，应该是只装 libmosquitto-dev 即可，为了安全把 libmosquitto1 也装上
$ # 如果是源码安装，千万记住安装后要重新执行 ./configure --with-mosquitto
$ sudo apt-get install libmosquitto1 libmosquitto-dev
```

接着用 PECL 安装 Mosquitto-PHP（用源码安装方式也一样，此处忽略）：
PECL最后一次更新是 *2017-03-13* 的 *0.4.0* 版本，参考 [Mosquitto](http://pecl.php.net/package/Mosquitto) 

```
$ pecl install Mosquitto
WARNING: channel "pecl.php.net" has updated its protocols, use "pecl channel-update pecl.php.net" to update
Cannot install, php_dir for channel "pecl.php.net" is not writeable by the current user
$ sudo pecl channel-update pecl.php.net
Updating channel "pecl.php.net"
Update of Channel "pecl.php.net" succeeded
$ sudo pecl install Mosquitto-alpha
...
Installing '/usr/lib/php/20151012/mosquitto.so'
install ok: channel://pecl.php.net/Mosquitto-0.4.0
configuration option "php_ini" is not set to php.ini location
You should add "extension=mosquitto.so" to php.ini
```

配置加载 mosquitto.so :

```
$ sudo cat /etc/php/7.0/cli/conf.d/20-mosquitto.ini
; configuration for php xml module
; priority=20
extension=mosquitto.so
```

### 选项3：纯PHP实现MQTT的 *composer* 库：[phpMQTT](https://packagist.org/packages/bluerhinos/phpmqtt)

缺点是代码简陋，功能不全。因此最后我把其源码拷贝下来，修改继续使用，不作为 composer 依赖。
其是纯 PHP 实现的，因此可以支持 Windows 。
修改参考文档：[MQTT协议－MQTT协议解析（MQTT数据包结构）](https://itbilu.com/other/relate/4ysLCoYvx.html) 

### References
* [MQTT - 360百科](https://baike.so.com/doc/7098130-7321082.html)
* [Simple Asynchronous Messaging](http://php.net/manual/zh/book.sam.php)
* [Install mosquitto on mac os x](http://www.xappsoftware.com/wordpress/2014/10/30/install-mosquitto-on-mac-os-x/)
* [https://github.com/mgdm/Mosquitto-PHP](https://github.com/mgdm/Mosquitto-PHP)
