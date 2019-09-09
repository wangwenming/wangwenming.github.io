# NewRelic

## APM - Application Performance Management

## New Relic

> The Java agent reports JVM statistics so that at any time you can see exactly how much memory is available/allocated, CPU usage, and time spent in garbage collection. Now you can measure multiple JVM resources in a single view.

> New Relic公司总部位于旧金山，提供绩效管理解决方案和应用程序性能管理SaaS，采用真实用户监控(RUM)技术。生产型服务器上的轻型代理软件可以将关于应用程序活动的数据发送到New Relic的数据中心，供分析工具和报告引擎立即处理。IT人员可以通过New Relic Web应用程序仪表板，定制视图、深入分析缓慢事务，并立即了解从最终用户的行为直到某一行代码的全部信息。

***NewRelic***启动方式如下：

```bash
$ java -Djava.security.egd=file:/dev/./urandom -javaagent:newrelic/newrelic.jar -jar -Xms256m -Xmx256m -XX:MaxMetaspaceSize=256m app.jar --spring.profiles.active=prod
```

### 随机数优化
> ***SecureRandom***在***Java***各种组件中使用广泛，可以可靠的产生随机数。但在大量产生随机数的场景下，性能会较低。
> 
> 这时可以使用```-Djava.security.egd=file:/dev/./urandom```加快随机数产生过程。<sup>[java.security.SecureRandom源码分析][java.security.SecureRandom源码分析]</sup>

> egd表示熵收集守护进程(entropy gathering daemon)

### 其他参数解释

```
java [ options ] -jar file.jar
  -Dproperty=value
    Sets a system property value.
  
  -javaagent:jarpath[=options]
    Load a Java programming language agent, see java.lang.instrument.
  
  -Xmsn
    Specifies the initial size of the memory allocation pool. 
    This value must be a multiple of 1024 greater than 1 MB. 
    The default value is 2MB.

  -XX:+UseAltSigs
    The VM uses SIGUSR1 and SIGUSR2 by default, which can sometimes conflict with applications that signal-chain SIGUSR1 and SIGUSR2.
    The -XX:+UseAltSigs option will cause the VM to use signals other than SIGUSR1 and SIGUSR2 as the default.
```

> Extended Memory Specification 扩展内存
> 
> Xms 是指设定程序启动时占用内存大小。一般来讲，大点，程序会启动的快一点，但是也可能会导致机器暂时间变慢。
> 
> Xmx 是指设定程序运行期间最大可占用的内存大小。如果程序运行需要占用更多的内存，超出了这个设置值，就会抛出OutOfMemory异常。
>
> Xss 是指设定每个线程的堆栈大小。这个就要依据你的程序，看一个线程大约需要占用多少内存，可能会有多少线程同时运行等。

## New Relic对Node的支持

[Node.js项目APM监控之New Relic][Node.js项目APM监控之New Relic]

## PinPoint

[Pinpoint技术概述][Pinpoint技术概述]

## References
* [java.security.SecureRandom源码分析][java.security.SecureRandom源码分析]
* [JVM上的随机数与熵池策略 - 云栖社区][JVM上的随机数与熵池策略 - 云栖社区]
* [JVM参数MetaspaceSize的误解][JVM参数MetaspaceSize的误解]
* [Node.js项目APM监控之New Relic][Node.js项目APM监控之New Relic]
* [Pinpoint技术概述][Pinpoint技术概述]

[java.security.SecureRandom源码分析]: https://blog.51cto.com/leo01/1795447 "java.security.SecureRandom源码分析"
[JVM上的随机数与熵池策略 - 云栖社区]: https://yq.aliyun.com/articles/20356 "JVM上的随机数与熵池策略 - 云栖社区"
[JVM参数MetaspaceSize的误解]: https://www.jianshu.com/p/b448c21d2e71 "JVM参数MetaspaceSize的误解"
[Node.js项目APM监控之New Relic]: https://www.cnblogs.com/kongxianghai/p/6841480.html "Node.js项目APM监控之New Relic"
[Pinpoint技术概述]: https://www.jianshu.com/p/a803571cd570 "Pinpoint技术概述"

