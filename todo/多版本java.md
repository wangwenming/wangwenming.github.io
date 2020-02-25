
```bash
$ java --version
openjdk 12.0.1 2019-04-16
OpenJDK Runtime Environment (build 12.0.1+12)
OpenJDK 64-Bit Server VM (build 12.0.1+12, mixed mode, sharing)
```

```bash
$ /usr/libexec/java_home -verbose
Matching Java Virtual Machines (2):
    12.0.1, x86_64:	"OpenJDK 12.0.1"	/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
    1.8.0_212, x86_64:	"AdoptOpenJDK 8"	/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
```

```
$ jenv version
1.8 (set by /Users/wenmingwang/.jenv/version)
```

将 ```jenv init -``` 的结果追加到 *~/.bash_profile* 文件。然后运行 ```jenv enable-plugin export``` 。

再运行：

```
$ jenv doctor
[OK]	No JAVA_HOME set
[OK]	Java binaries in path are jenv shims
[OK]	Jenv is correctly loaded
```

*Tooling API* 是 *Gradle* 提供给 IDE(例如 [Eclipse Buildship](http://projects.eclipse.org/projects/tools.buildship)) 用来嵌入(embedding) Gradle 的 API 。

```
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home

# /usr/libexec/java_home 符号链接到下面命令 
$ /System/Library/Frameworks/JavaVM.framework/Versions/A/Commands/java_home
/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home

$ /System/Library/Frameworks/JavaVM.framework/Versions/A/Commands/java -version
openjdk version "12.0.1" 2019-04-16
```

```
~/.gradle/wrapper/dists/gradle-2.10-bin/baigpnfu14tdk6ztbfwcl8275/
```


```/usr/libexec/java_home``` 是一个二进制文件。


$ brew cask uninstall java
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

如果 ```/usr/libexec/java_home``` 命令总是指向 java12 ，那么 本地 gradle 也是指向 java12

```
$ cat /usr/local/bin/gradle
#!/bin/bash
JAVA_HOME="${JAVA_HOME:-$(/usr/libexec/java_home)}" exec "/usr/local/Cellar/gradle/5.5.1/libexec/bin/gradle" "$@"
```