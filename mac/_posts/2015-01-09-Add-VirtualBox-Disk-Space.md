### 第一步：把 VirtualBox 磁盘扩大
```VBoxManage modifyhd ~/VirtualBox\ VMs/Win7/Win7.vdi --resize 30000 # 30GB```
多出来的磁盘是未分配

### 第二步：把未分配的磁盘空间合并到C盘
```开始菜单的计算机 -> 右键 -> 管理(G) -> 存储-磁盘管理 -> 点击C盘，右键 -> 扩展卷(X)```

http://www.2cto.com/os/201205/132630.html VirtualBox磁盘扩容的方法
http://www.3lian.com/edu/2012/12-03/47440.html Win7系统下扩大C盘容量的方法