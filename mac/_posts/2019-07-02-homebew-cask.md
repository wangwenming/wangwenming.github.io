### 简介

***Homebrew***是***Ruby***开发的。

### 常用命令

```bash
# 查看版本
$ $ brew --version
Homebrew 2.1.6
Homebrew/homebrew-core (git revision 825f; last commit 2019-07-02)
Homebrew/homebrew-cask (git revision 7f5f8; last commit 2019-07-01)

# 更新 Homebrew
$ brew update

# 查看已安装的包
$ brew list

# 搜索包
$ brew search php
php
php@7.1
php@7.2

# 查看信息
$ brew info php
php: stable 7.3.6 (bottled)

$ brew cask info java6
Error: Cask java6 exists in multiple taps:
  homebrew/cask-versions/java6
  caskroom/versions/java6
  
# 不要再用 caskroom 了（由于历史原因，从一些教程上学的，加了caskroom，理应去掉）
$ brew untap caskroom/cask
$ brew untap caskroom/versions
```

```bash
 brew doctor                        # 自检
 brew update                        # 更新brew可安装包，建议每次执行一下，会同时更新 Homebrew 本身
 brew search php55                  # 搜索php5.5
 brew tap josegonzalez/php          # 安装扩展<gihhub_user/repo>   
 brew tap                           # 查看安装的扩展列表
 brew install php55                 # 安装php5.5
 brew remove  php55                 # 卸载php5.5
 brew upgrade php55                 # 升级php5.5
 brew options php55                 # 查看php5.5安装选项
 brew info    php55                 # 查看php5.5相关信息
 brew home    php55                 # 访问php5.5官方网站
 brew services list                 # 查看系统通过 brew 安装的服务
 brew services cleanup              # 清除已卸载无用的启动配置文件
 brew services restart php55        # 重启php-fpm
```

### Taps (third-party repositories)

查看第三方 Tap（按2019-07-02 Homebrew 2.1.6，默认就是如下几个，够了）

```bash
$ brew tap
homebrew-cask
homebrew-cask-versions
homebrew-core
homebrew-services
```

添加第三方 Tap

```bash
$ brew tap <user/repo> 
```

***备注*** ```Homebrew/versions``` 已被废弃，因为 ```Homebrew/Core``` 已支持多版本。```homebrew/dupes``` 也一样，甚至连 github 都 404 了。

### Cask Tap(2019-07-02 Homebrew 2.1.6是默认安装)

> Homebrew Cask extends Homebrew and brings its elegance, simplicity, and speed to the installation and management of ***GUI*** macOS applications such as ***Atom*** and ***Google Chrome***.

命令基本都是 ```brew cask``` 开始。搜索还是用 ```brew search``` ，搜出的结果如：

```bash
==> Casks
google-chrome
homebrew/cask-versions/google-chrome-beta
homebrew/cask-versions/google-chrome-dev
homebrew/cask-versions/google-chrome-canary
```

```bash
# 查看已安装的Cask
$ brew cask list
java

# 查看已安装的和未安装的Cask的详情

$ brew cask info java
java: 12.0.1,69cfe15208a647278a19ef0990eea691
https://www.oracle.com/technetwork/java/javase/
/usr/local/Caskroom/java/12.0.1,69cfe15208a647278a19ef0990eea691 (64B)
From: https://github.com/Homebrew/homebrew-cask/blob/master/Casks/java.rb
==> Name
OpenJDK Java Development Kit
==> Artifacts
jdk-12.0.1.jdk -> /Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk (Generic Artifact)

$ brew cask info google-chrome
google-chrome: 75.0.3770.100 (auto_updates)
https://www.google.com/chrome/
Not installed
From: https://github.com/Homebrew/homebrew-cask/blob/master/Casks/google-chrome.rb
==> Name
Google Chrome
==> Artifacts
Google Chrome.app (App)
```

```brew update``` 一并更新 ***Homebrew Cask***

### brew 和 brew cask 的区别

> brew 装的主要是 command line tool。

> brew cask装的大多是有gui界面的app以及驱动，brew cask是brew的一个官方源。

另一篇解释：

> ***brew*** 是从软件包仓库下载源代码码到本地进行解压，进而执行 ```./configure && make install``` 将软件编译安装到单独的目录 ```/usr/local/Cellar``` 下，然后软链到 ```/usr/local``` 目录下，同时会自动检测下载相关依赖库，并自动配置好各种环境变量。

> 而 ***brew cask*** 是已经编译好了的应用包(```.dmg``` 或者 ```.pkg```)，仅仅是下载解压，放在统一的目录中(```/usr/local/Caskroom/```)，省掉了自己去下载、解压、拖拽（安装）等蛋疼步骤，同样，卸载相当容易与干净。这个对一般用户来说会比较方便，包含很多在 AppStore 里没有的常用软件。

Cask有两个源头：

* https://github.com/Homebrew/homebrew-cask 
* https://github.com/Homebrew/homebrew-cask-versions

2019-07-02(Homebrew 2.1.6)实测知乎文档说的Git已迁移，访问旧项目会301到新项目：

| 知乎文档Git | 现在的Git |
| ---------- | -------- |
| [phinze/homebrew-cask](https://github.com/phinze/homebrew-cask) | [Homebrew/homebrew-cask](https://github.com/Homebrew/homebrew-cask) |
| [caskroom/homebrew-versions](https://github.com/caskroom/homebrew-versions) | [Homebrew/homebrew-cask-versions](https://github.com/Homebrew/homebrew-cask-versions) |

### 更多 Taps

| Tap name | description |
|:-------- |:----------- |
| Homebrew/cask-versions | contains alternate versions of Casks (e.g. betas, nightly releases, old versions) | 
| Homebrew/cask-fonts | contains Casks that install fonts |

最重要的就是 ```Homebrew/cask-versions``` 了，因为 MySQL、Java 等等都需要安装老版本。通过 ```brew tap homebrew/cask-versions``` 就可以添加第三方 Tap 。

### 安装 Java8

> Oracle 对 Java 8 的官方支持时间持续到 2020 年 12 月，之后将不再为个人桌面用户提供 Oracle JDK 8 的修复更新；在 2019 年 1 月之后，不再提供免费的商业版本更新，届时想要继续获得 Oracle 的商业支持和维护，需付费订阅。

细节参考 [Java8 not working anymore #7253](https://github.com/Homebrew/homebrew-cask-versions/issues/7253) 

```bash
# brew不包含 Java，Cask 提供了 java6(Apple Java 6) 和 Java11 Java (默认的Java12版本) java-beta  3个 OpenJDK 选项
$ brew search java
==> Casks
java java11 java6 java-beta

$ brew search jdk
adoptopenjdk adoptopenjdk8 oracle-jdk
$ brew cask info adoptopenjdk
adoptopenjdk: 12.0.1,12
...
$ brew cask info oracle-jdk
oracle-jdk: 12.0.1,12
...
$ brew cask info homebrew/cask-versions/adoptopenjdk8
adoptopenjdk8: 8,212:b04
...
OpenJDK8U-jdk_x64_mac_hotspot_8u212b04.pkg (Pkg)
$ brew cask install adoptopenjdk8

$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
$ ls -l /Library/Java/JavaVirtualMachines
adoptopenjdk-8.jdk
openjdk-12.0.1.jdk
$ brew install jenv
$ export PATH="$HOME/.jenv/bin:$PATH"
$ eval "$(jenv init -)"
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
$ jenv add /Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
$ jenv global 1.8
$ java -version
openjdk version "1.8.0_212"

# 这个还没有变
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/openjdk-12.0.1.jdk/Contents/Home
```

### 附录：术语
```brew /brʊ/``` => vt. 酿造；酝酿

```homebrew``` => n. 自酿（啤）酒；公司自产自用

	通常翻译为“自制软件”。其 Logo 是一杯啤酒上面一个苹果。

```formula /'fɔrmjələ/``` => n. [数] 公式，准则；配方；婴儿食品

	例：the secret formula for the blending of the whisky 调配威士忌的秘方
	例：Formula One[TM] racing 一级方程式赛车

```cask /kæsk/``` => n. 木桶，桶

### References

* [https://brew.sh/](https://brew.sh/)
* [How to Use Homebrew Cask](https://github.com/Homebrew/homebrew-cask/blob/master/USAGE.md)
* [Homebrew Documentation - Versions](https://docs.brew.sh/Versions.html)
* [brew和brew cask有什么区别？](https://www.zhihu.com/question/22624898)
* [AdoptOpenJDK/homebrew-openjdk](https://github.com/AdoptOpenJDK/homebrew-openjdk)
* [jEvn](https://www.jenv.be/)
