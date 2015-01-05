---
category: android
---

#现象
```
03-03 15:17:43.909: E/Trace(18168): error opening trace file: No such file or directory (2)
03-03 15:17:43.959: W/dalvikvm(18168): Refusing to reopen boot DEX '/system/framework/hwframework.jar'
```

#方法
手机拨号*#*#2846579#*#*，进入 projectmenu -> 后台设置 -> LOG设置 -> LOG开关 -> 打开

#参考
1. [logcat] http://blog.csdn.net/gebitan505/article/details/14453231
    "华为手机在开发Android调试时logcat不显示输出信息的解决办法"
