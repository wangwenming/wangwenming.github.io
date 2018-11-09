## How to View & Remove Extended Attributes from a File on Mac OS

### 序
因为我的U盘文件格式是 **DOS_FAT_32** ，从 **Mac** 拷贝经常报 **chflags** 失败，无法拷贝。发现是 **Mac** 的文件扩展属性导致的。解决办法就是把扩展属性清除。

### 什么是 Mac 的扩展属性（Extended Attributes）

用来存储元数据(metadata)，可供 **Spotlight** 和 **Finder** 等工具使用。

例如查找所有屏幕截图，可以在 **Finder** 的 **Search** 框输入```kMDItemIsScreenCapture:1```即可。

### xattr内置命令

* 查看某个文件的扩展属性

```
$ xattr navigator.geolocation.png
com.apple.FinderInfo
com.apple.lastuseddate#PS
com.apple.metadata:kMDItemIsScreenCapture
com.apple.metadata:kMDItemScreenCaptureGlobalRect
com.apple.metadata:kMDItemScreenCaptureType
```
* 删除某个文件的(单个)扩展属性

```
$ xattr -d com.apple.FinderInfo navigator.geolocation.png
$ xattr -d com.apple.lastuseddate#PS navigator.geolocation.png
$ xattr -d com.apple.metadata:kMDItemIsScreenCapture navigator.geolocation.png
$ xattr -d com.apple.metadata:kMDItemScreenCaptureGlobalRect navigator.geolocation.png
$ xattr -d com.apple.metadata:kMDItemScreenCaptureType navigator.geolocation.png
```

* 清除某个文件的(所有)扩展属性

```
$ xattr -c navigator.geolocation.png
```

### 递归删除目录下所有扩展属性
这个命令可以删除所有：

```
$ find ~/Documents/项目 -xattr -print0 | xargs -0 xattr -c
```

*备注* 

如何解决文件中有空格的情况？查看 *find* 文档：

> However, you may wish to consider the **-print0** primary in conjunction with ''**xargs -0**'' as an effective alternative.


### References
1. [How to View & Remove Extended Attributes from a File on Mac OS
](http://osxdaily.com/2018/05/03/view-remove-extended-attributes-file-mac/) - 2018-05-03

