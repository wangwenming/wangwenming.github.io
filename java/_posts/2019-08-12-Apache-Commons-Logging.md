# Apache Commons Logging

全称是 ***Jakarta Commons Logging API*** ，简称 ***JCL*** 。官网是 [The Logging Component](http://commons.apache.org/proper/commons-logging/) ，最新版本是 ***2014-07-09*** 发布的 ***v1.2*** 版本。

> Is a thin, modular bridging API with out-of-the-box support for most well known logging systems.<sup>[Common Logging wiki][wiki]</sup>

## 为什么要用***JCL***

***一句话：*** library要使用***JCL***，application可以不使用。

> When writing a library it is very useful to log information. However there are many logging implementations out there, and a library cannot impose the use of a particular one on the overall application that the library is a part of.<sup>[The Logging Component][The Logging Component]</sup>

写一个 lib 的时候会用到 log 功能，但是又不能指定使用哪个，因为这个 lib 只是应用程序的一部分，要让应用程序来选择一个统一的。

> Applications (rather than libraries) may also choose to use commons-logging. While logging-implementation independence is not as important for applications as it is for libraries, using commons-logging does allow the application to change to a different logging implementation without recompiling code.<sup>[The Logging Component][The Logging Component]</sup>

应用程序使用 ***commons-logging*** 也有好处，可以不用重新编译就切换 log 实现。

> Apache Tomcat does this; the person installing it can choose to use log4j, java.util.logging, etc. There is some debate over how useful this actually is.<sup>
> 
> Commons-logging can also be useful when you are writing an application but want to reserve the right to swap logging libraries at a later date. This is not so important, though, as it really isn't that hard to use some refactoring tool or a few scripts to change source code from using library X to library Y.<sup>[Common Logging wiki][wiki]</sup>

## ***JCL***的实现
* java.util.logging
* Log4J
* Simple Log
* [Logback](https://logback.qos.ch/) - Log4J作者的新作 (successor to the popular log4j project)

## ***Logback***简介

是瑞士洛桑的一家Java软件公司 ***Quality Open Software*** 开发的（***.ch***取自瑞士的拉丁文名字Confoederatio Helvetica）。

Logback分成3个模块：

* logback-core
* logback-classic
* logback-access

> The logback-core module lays the groundwork for the other two modules. The logback-classic module can be assimilated to a significantly improved version of log4j. Moreover, logback-classic natively implements the [SLF4J API](http://www.slf4j.org/) so that you can readily switch back and forth between logback and other logging frameworks such as log4j or java.util.logging (JUL).<sup>[Logback Home][Logback Home]</sup>

## ***Logback***功能详解

### ```<logger>```与```<root>```

```<logger>```用来设置某一个包或者具体某一个类的日志打印级别、以及指定```<appender>```。```<logger>```可以包含零个或者多个```<appender-ref>```元素，标识这个***appender***将会添加到这个***logger***。```<logger>```仅有一个***name***属性、一个可选的***level***属性和一个可选的***additivity***属性：

```<root>```也是```<logger>```元素，但是它是***根logger***，只有一个***level***属性，因为它的***name***就是ROOT。

~~~xml
<!-- 
1. <logger>中没有配置level，即继承父级的level，<logger>的父级为<root>，那么level=debug
2. 没有配置additivity，那么additivity=true，表示此<logger>的打印信息向父级<root>传递
3. 没有配置<appender-ref>，表示此<logger>不会打印出任何信息
-->
<logger name="java" />
<root level="debug">
    <appender-ref ref="STDOUT" />
</root>
~~~

```name="java"```是```name="java.lang"```的父级，```<root>```是所有```<logger>```的父级。

### ```<appender>```

~~~xml
<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger - %msg%n</pattern>
    </encoder>
</appender>
~~~

```<encoder>```是***0.9.19***版本之后引进的，以前的版本使用```<layout>```，***Logback***极力推荐的是使用```<encoder>```而不是```<layout>```。

```<appender>```是```<configuration>```的子节点，是负责写日志的组件。```<appender>```有两个必要属性***name***和***class***：

* ***name*** —— 指定```<appender>```的名称
* ***class*** —— 指定```<appender>```的全限定名

***class***默认实现有：

* ```ch.qos.logback.core.ConsoleAppender``` —— 输出到控制台
* ```ch.qos.logback.core.FileAppender``` —— 输出到文件
* ```ch.qos.logback.core.rolling.RollingFileAppender``` —— 输出到文件并实现rolling
* ```ch.qos.logback.classic.AsyncAppender``` —— 异步输出

### ```<encoder>```
```<encoder>```节点负责两件事情：
* 把日志信息转换为字节数组
* 把字节数组写到输出流

目前***PatternLayoutEncoder***是唯一有用的且默认的***encoder***，有一个```<pattern>```节点，就像上面演示的，用来设置日志的输入格式。

[Logback PatternLayoutEncoder语法][Logback PatternLayoutEncoder语法]：

转换符 | 示例 | 作用
----- | --- | ----
%date{pattern} | %date{yyyy-MM-dd HH:mm:ss.SSS} | 输出时间格式，模式语法同java.text.SimpleDateFormat兼容
p / le / level | %-5level | 输出日志级别，-5表示使用5个字符靠左对齐
t / thread | | 输出产生日志的线程名称
C{length} / class{length} | %class | Outputs the fully-qualified class name of the caller issuing the logging request
L / line | %line | Outputs the line number from where the logging request was issued
m / msg / message | %msg | 日志内容
n | %n | 平台相关的换行符
 X{key:-defaultVal} / mdc{key:-defaultVal} | [%X{sessionTokenId}] | Outputs the MDC (mapped diagnostic context) associated with the thread that generated the logging event.

## Spring Boot 配置 ***Logback***

默认情况下，Spring Boot会用Logback来记录日志，并用INFO级别输出到控制台。

使用 ***Gradle*** 引入 ***Logback***：
```
compile("org.springframework.boot:spring-boot-starter-logging")
```

日志级别从低到高分为```TRACE``` < ```DEBUG``` < ```INFO``` < ```WARN``` < ```ERROR``` < ```FATAL```，如果设置为```WARN```，则低于```WARN```的信息都不会输出。

Spring Boot中***Logback***配置文件名默认为以下：

* ```logback-spring.xml```
* ```logback-spring.groovy```
* ```logback.xml```
* ```logback.groovy```

Spring Boot官方推荐优先使用带有```-spring```的文件名作为你的日志配置，如使用```logback-spring.xml```，而不是```logback.xml```。命名为```logback-spring.xml```的日志配置文件，Spring Boot可以为它添加一些Spring Boot特有的配置项。

如果你即想完全掌控日志配置，但又不想用```logback.xml```作为***Logback***配置文件名，可以通过```logging.config```属性自定义文件名。

例如会找房项目：
```curl http://conf.huizhaofang.com/hzf-platform-application/production/master```

通过配置```logging.config```属性指定***Logback***配置文件名。

```
logging.config: "config/prod-logback.xml"
```

## What alternatives are there to commons-logging?

[SLF4J](http://slf4j.org) 

## References
* [The Logging Component][The Logging Component]
* [Common Logging wiki][wiki]
* [Logback Home][Logback Home]
* [Java日志框架：logback详解][Java日志框架：logback详解]
* [Logback PatternLayoutEncoder语法][Logback PatternLayoutEncoder语法]

[The Logging Component]: http://commons.apache.org/proper/commons-logging/ "The Logging Component"
[wiki]: https://cwiki.apache.org/confluence/display/commons/Logging "Common Logging"
[Logback Home]: https://logback.qos.ch/ "Logback Home"
[Java日志框架：logback详解]: https://www.cnblogs.com/xrq730/p/8628945.html "Java日志框架：logback详解"
[Logback PatternLayoutEncoder语法]: https://logback.qos.ch/manual/layouts.html#conversionWord "Logback PatternLayoutEncoder语法"

