
参考文档1：

    猴子补丁（Monkey Patch）是一种特殊的编程技巧。
    Monkey patch 可以用来在运行时动态地修改（扩展）类或模块。
    我们可以通过添加 Monkey Patch 来修改不满足自己需求的第三方库，
    也可以添加 Monkey Patch 临时修改代码中的错误。

参考文档2：

    A monkey patch is a way for a program to extend or modify 
    supporting system software locally 
    (affecting only the running instance of the program).

早期叫 guerrilla patch 游击队员补丁?，形容这种补丁像游击队员一样狡猾，是一种runtime 补丁。 guerrilla [ɡə'rɪlə] 和 gorilla [ɡə'rɪlə] (大猩猩) 同音，后来就变成“猴子补丁”了。

典型的就是JS的各种 shim 和 polyfill 了，如 es5-shim 。

### References
1. [Ruby使用Monkey Patch猴子补丁方式进行程序开发的示例](http://www.jb51.net/article/85263.htm)
2. [Monkey patch](https://en.wikipedia.org/wiki/Monkey_patch)