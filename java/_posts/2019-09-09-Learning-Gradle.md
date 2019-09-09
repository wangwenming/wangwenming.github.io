# Learning Gradle

## dependencies

### Project dependencies

```
dependencies {
    // This dependency is used by the application.
    implementation 'com.google.guava:guava:27.1-jre'
}
```

## the Java plugin
引入了：

* dependency configurations
* Tasks: compileJava test

### Java插件引入的依赖配置（Dependency configurations）
* <del>compile</del> (Deprecated) 编译时依赖，被 ***implementation*** 替代
* implementation extends compile - Implementation only dependencies.
* compileOnly - Compile time only dependencies, not used at runtime.
* <del>runtime</del> (Deprecated) extends compile - Runtime dependencies. Superseded by runtimeOnly.
* runtimeOnly - Runtime only dependencies.
* <del>testCompile</del>(Deprecated) extends compile - Additional dependencies for compiling tests. Superseded by testImplementation.
* testImplementation extends testCompile, implementation - Implementation only dependencies for tests.

### Java插件的Tasks
* ***compileJava*** - Compiles production Java source files using the JDK compiler.
* ***processResources*** - Copies production resources into the production resources directory.
* ***classes*** - aggregate task: ***compileJava*** + ***processResources***
* ***jar*** - 依赖 ***classes*** ，打包成 ***jar*** 文件
* ***javadoc*** - Generates API documentation for the production Java source using Javadoc.

### Java插件的配置
```
# The Java language level to use to compile the source files.
sourceCompatibility = 1.8
# The target JVM to generate the .class files for.
targetCompatibility = 1.8
```

## [Base Plugin](https://docs.gradle.org/current/userguide/base_plugin.html)

## Repositories

内置了3个 ***shortcut***：

* mavenCentral()
* jcenter()
* google() - Google Android repository

Declaring a custom repository by URL：
```
repositories {
    maven {
      url 'http://repo.mycompany.com/maven2'
    }
}
```

## Eclipse Plugin
生成 ```.project``` 文件并配置Java、生成 ```.classpath``` 文件。

## References
* [Building Java Web Applications](https://guides.gradle.org/building-java-web-applications/)
* [Declaring Repositories](https://docs.gradle.org/current/userguide/declaring_repositories.html#header)
* [Building Java Projects with Gradle](https://spring.io/guides/gs/gradle/)
* [The Eclipse Plugins](https://docs.gradle.org/current/userguide/eclipse_plugin.html)
* [Building Spring Boot 2 Applications with Gradle](https://guides.gradle.org/building-spring-boot-2-projects-with-gradle/)
* [Build Script Basics](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)

