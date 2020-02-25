# 替换 Gradle 默认的构建脚本仓库解决无法访问的问题和提速

## 问题
Gradle 默认的构建脚本仓库是 [https://plugins.gradle.org/m2/](https://plugins.gradle.org/m2) 在国内经常访问不了，这样就玩不下去了。

## 解决方案

因此需要自定义 Gradle 构建脚本仓库，替换默认值，编辑 ```build.gralde``` 文件，新增如下代码：

```
# 1. 必须在 plugins {} blocks 前
# 2. 注意 buildscript 是全小写，正确的时候 buildscript 会高亮，错误了不会提示，被忽略了
buildscript {
	repositories {
		maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
    mavenCentral()
	}
}
```

## ```buildscript.repositories``` 和 ```repositories``` 的区别
大家经常会看到项目中定义了两次：

```
buildscript {
	repositories {
		maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
    mavenCentral()
	}
}

repositories {
  maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
  mavenCentral()
}
```

那么是重复了么？显然不是，```buildscript.repositories``` 指的是 ```build.gralde``` 构建脚本自身的依赖仓库，```repositories``` 指的是 构建项目代码的依赖仓库。