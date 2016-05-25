## Tomcat单机部署方案
### copying web application archive file (.war)
### copying unpacked web application directory
### using Tomcat’s manager application

### Session共享
方式一：使用 Tomcat 自带的 Session Replication 功能(in-memory-replication)在 Cluster 内的 Tomcat 实例间，通过 TCP 广播 Session 并共享。
1. DeltaManager: all-to-all replication; efficient when the clusters are small.
2. BackupManager: the session will only be stored at one backup server simply setup the BackupManager; For larger clusters.
方式二：使用外部 Session 存储(PersistenceManager + FileStore/JDBCStore)，如 Redis。


## Reference
http://tomcat.apache.org/tomcat-7.0-doc/appdev/deployment.html
http://www.codejava.net/servers/tomcat/how-to-deploy-a-java-web-application-on-tomcat
https://tomcat.apache.org/tomcat-7.0-doc/config/cluster-deployer.html

http://tomcat.apache.org/tomcat-7.0-doc/cluster-howto.html#For_the_impatient

# 重启是用远程命令，逐个重启吗？