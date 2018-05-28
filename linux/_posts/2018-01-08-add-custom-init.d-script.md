## 常见软件安装时，包管理工具会自动安装 Service 脚本
例如， *MySQL* 在 CentOS 下的脚本： ```/etc/init.d/mysqld```
并且有如下符号链接，定义不同的 *runlevel* 下 *MySQL* 的运行状态 ：
```
/etc/rc0.d/K36mysqld -> ../init.d/mysqld
/etc/rc1.d/K36mysqld -> ../init.d/mysqld
/etc/rc2.d/K36mysqld -> ../init.d/mysqld
/etc/rc3.d/S64mysqld -> ../init.d/mysqld
/etc/rc4.d/S64mysqld -> ../init.d/mysqld
/etc/rc5.d/S64mysqld -> ../init.d/mysqld
/etc/rc6.d/K36mysqld -> ../init.d/mysqld
```

## 如何添加自定义的 Service 脚本

### 查看当前所有 Service 的运行状态
```
$ sudo service --status-all
```

### Linux的7种运行级(runlevel)：

停机、单用户工作状态、多用户状态(没有NFS)、完全的多用户状态(有NFS)、X11（带UI界面）、重启

详细解释如下：

* 0 - halt (Do NOT set initdefault to this) 系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
* 1 - Single user mode 单用户工作状态，root权限，用于系统维护，禁止远程登陆
* 2 - Multiuser, without NFS (The same as 3, if you do not have networking)
* 3 - Full multiuser mode 完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
* 4 - unused 系统未使用，保留
* 5 - X11 X11控制台，登陆后进入图形GUI模式
* 6 - reboot (Do NOT set initdefault to this) 系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

### 运行级别的原理
1. 在目录/etc/rc.d/init.d (rc: ```run commands``` 缩写) 下有许多服务器脚本程序，一般称为服务(service)
2. 在/etc/rc.d下有7个名为rcN.d的目录，对应系统的7个运行级别
3. rcN.d目录下都是一些符号链接文件，这些链接文件都指向init.d目录下的service脚本文件，命名规则为K+nn+服务名或S+nn+服务名，其中nn为两位数字，2位数字表示优先级，越小的越优先。
4. 系统会根据指定的运行级别进入对应的rcN.d目录，并按照文件名顺序检索目录下的链接文件
对于以K (kill) 开头的文件，系统将终止对应的服务
对于以S (Start) 开头的文件，系统将启动对应的服务
5. 查看运行级别用：runlevel
6. 进入其它运行级别用：init N
7. ```init 0``` 为关机，```init 6``` 为重启系统
8. runlevel 服务器的话，是 3 ，在文件 /etc/inittab 最后一行，如：```id:3:initdefault:``` 可以修改

### chkconfig 配置
chkconfig --list 显示当前所有 service 在每个层级的运行状态

脚本文件中如下几行“注释”，其实这只是“bash的注释”， *chkconfig* 程序会读取这几行信息：

```
# description: Apache Tomcat init script
# processname: tomcat
# chkconfig: 234 20 80
```

其中 ```chkconfig: 234 20 80```  表示在级别 2 3 4 下开启。

```bash
# 添加好就大功告成了，可手动启动一下
$ sudo chkconfig --add tomcat
# 删除
$ sudo chkconfig --del tomcat
```

### 附：tomcat 的 Service 脚本：
```
#!/bin/bash
#
# description: Apache Tomcat init script
# processname: tomcat  
# chkconfig: 234 20 80  
#
#
# Copyright (C) 2014 Miglen Evlogiev
#
# This program is free software: you can redistribute it and/or modify it under
# the terms of the GNU General Public License as published by the Free Software
# Foundation, either version 3 of the License, or (at your option) any later
# version.
#
# This program is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of  MERCHANTABILITY or FITNESS
# FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License along with
# this program.  If not, see <http://www.gnu.org/licenses/>.
#
# Initially forked from: gist.github.com/valotas/1000094
# Source: gist.github.com/miglen/5590986

#CATALINA_HOME is the location of the bin files of Tomcat  
export CATALINA_HOME=/home/ec/tomcat7/bin

#CATALINA_BASE is the location of the configuration files of this instance of Tomcat
export CATALINA_BASE=/home/ec/tomcat7/conf

#TOMCAT_USER is the default user of tomcat
export TOMCAT_USER=ec

#TOMCAT_USAGE is the message if this script is called without any options
TOMCAT_USAGE="Usage: $0 {\e[00;32mstart\e[00m|\e[00;31mstop\e[00m|\e[00;31mkill\e[00m|\e[00;32mstatus\e[00m|\e[00;31mrestart\e[00m}"

#SHUTDOWN_WAIT is wait time in seconds for java proccess to stop
SHUTDOWN_WAIT=20

tomcat_pid() {
        echo `ps -fe | grep $CATALINA_BASE | grep -v grep | tr -s " " | cut -d" " -f2`
}

start() {
  pid=$(tomcat_pid)
  if [ -n "$pid" ]
  then
    echo -e "\e[00;31mTomcat is already running (pid: $pid)\e[00m"
  else
    # Start tomcat
    echo -e "\e[00;32mStarting tomcat\e[00m"
    #ulimit -n 100000
    #umask 007
    #/bin/su -p -s /bin/sh $TOMCAT_USER
        if [ `user_exists $TOMCAT_USER` = "1" ]
        then
                /bin/su $TOMCAT_USER -c $CATALINA_HOME/bin/startup.sh
        else
                echo -e "\e[00;31mTomcat user $TOMCAT_USER does not exists. Starting with $(id)\e[00m"
                sh $CATALINA_HOME/bin/startup.sh
        fi
        status
  fi
  return 0
}

status() {
          pid=$(tomcat_pid)
          if [ -n "$pid" ]
            then echo -e "\e[00;32mTomcat is running with pid: $pid\e[00m"
          else
            echo -e "\e[00;31mTomcat is not running\e[00m"
            return 3
          fi
}

terminate() {
    echo -e "\e[00;31mTerminating Tomcat\e[00m"
    kill -9 $(tomcat_pid)
}

stop() {
  pid=$(tomcat_pid)
  if [ -n "$pid" ]
  then
    echo -e "\e[00;31mStoping Tomcat\e[00m"
    #/bin/su -p -s /bin/sh $TOMCAT_USER
        sh $CATALINA_HOME/bin/shutdown.sh

    let kwait=$SHUTDOWN_WAIT
    count=0;
    until [ `ps -p $pid | grep -c $pid` = '0' ] || [ $count -gt $kwait ]
    do
      echo -n -e "\n\e[00;31mwaiting for processes to exit\e[00m";
      sleep 1
      let count=$count+1;
    done

    if [ $count -gt $kwait ]; then
      echo -n -e "\n\e[00;31mkilling processes didn't stop after $SHUTDOWN_WAIT seconds\e[00m"
      terminate
    fi
  else
    echo -e "\e[00;31mTomcat is not running\e[00m"
  fi

  return 0
}

user_exists(){
        if id -u $1 >/dev/null 2>&1; then
        echo "1"
        else
                echo "0"
        fi
}

case $1 in
    start)
      start
    ;;
    stop)  
      stop
    ;;
    restart)
      stop
      start
    ;;
    status)
        status
        exit $?  
    ;;
    kill)
        terminate
    ;;      
    *)
        echo -e $TOMCAT_USAGE
    ;;
esac    
exit 0
```

## References
[9. Starting Your Software Automatically on Boot](http://www.tldp.org/HOWTO/HighQuality-Apps-HOWTO/boot.html)
[chkconfig - Unix, Linux Command](https://www.tutorialspoint.com/unix_commands/chkconfig.htm)
[chkconfig(8) - Linux man page](https://linux.die.net/man/8/chkconfig)

