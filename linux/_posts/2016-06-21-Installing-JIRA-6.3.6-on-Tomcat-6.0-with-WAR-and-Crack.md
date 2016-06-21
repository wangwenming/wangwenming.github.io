# Linux下通过WAR包方式安装Jira 3.6.3到Tomcat6，以及破解、数据备份和恢复

## 环境
* Mac OS X 10.10.5 (Yosemite)
* Tomcat 6.0.44
* Oracle Java 1.7.0_79
* MySQL 5.6.29
* MySQL Connector Java mysql-connector-java-5.1.39

## 下载WAR包
从[JIRA Downloads Archive](https://www.atlassian.com/software/jira/download-archives)下载对应的版本，如本次安装使用的是[6.3.6 WAR (TAR.GZ Archive)](https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-6.3.6-war.tar.gz)

## 安装步骤
### 解压缩
解压缩后的目录叫JIRA安装目录(JIRA Installation Directory)。<br>
**注意**：Windows下推荐使用7-zip工具解压缩，对长文件名有更好的支持。

### 修改文件
JIRA安装目录下有*webapp*、*edit-webapp*两个目录，只应该修改*edit-webapp*目录下的文件，默认已有2个文件，如`WEB-INF/classes/jira-application.properties`和`WEB-INF/classes/entityengine.xml`。如果要修改其他文件，从*webapp*目录拷贝到*edit-webapp*后修改。

在`WEB-INF/classes/jira-application.properties`文件设置*jira.home = 创建好的一个空文件夹*，临时文件、上传的附件等都放在这个目录。
注意不要和JIRA安装目录一样。


同时设置环境变量*JIRA_HOME*，等于*jira.home*。

### 配置*entityengine.xml*
无需修改

### 添加JDBC驱动
从[Download Connector/J](http://dev.mysql.com/downloads/connector/j/)下载最新的，解压后把jar文件拷贝到JIAR安装目录下的`webapp/WEB-INF/lib`目录。

### 其他Tomcat需要的lib
从[Tomcat 6 JARs](http://www.atlassian.com/software/jira/downloads/binary/jira-jars-tomcat-distribution-6.4-m12-tomcat-6x.zip)下载，解压后也放到JIAR安装目录下的`webapp/WEB-INF/lib`目录。

### 运行*build.sh*
运行完了会在生成如`dist-tomcat/tomcat-6/atlassian-jira-6.3.6.war`的文件，这个文件就是我们要部署到Tomcat的包。

### 配置*Tomcat*的*Context*
`conf/Catalina/localhost/jira.xml`文件包含如下内容：

```
<Context path="/jira" docBase="$JIRAInstallationDirectory/dist-tomcat/tomcat-6/atlassian-jira-6.3.6.war" reloadable="false">
    <Resource name="UserTransaction" auth="Container" type="javax.transaction.UserTransaction"
      factory="org.objectweb.jotm.UserTransactionFactory" jotm.timeout="60"/>
    <Manager pathname=""/>
</Context>
```

### 破解
首先翻墙注册一个atlassion账号。
然后一路初始化，在*License Key*那个步骤，选择“我有账户但没密钥”，会自动生成一个。
可以去[MyAtlassion](https://my.atlassian.com/product)拷贝出来。
停止Tomcat、下载[atlassian-extras-2.2.2.jar](http://77g9rk.com1.z0.glb.clouddn.com/@/jira/atlassian-extras-2.2.2.jar)并覆盖Tomcat目录下的`webapp/WEB-INF/lib/atlassian-extras-2.2.2.jar`文件。启动Tomcat。

手动写如下Key（记得替换成自己的字段）：

```
Description=JIRA: Commercial,
CreationDate=2016-06-17,
jira.LicenseEdition=ENTERPRISE,
Evaluation=false,
jira.LicenseTypeName=COMMERCIAL,
jira.active=true,
licenseVersion=2,
MaintenanceExpiryDate=2099-12-31,
Organisation=TianYao,
SEN=SEN-L8065086,
ServerID=BG32-87YV-F8TB-PVMX,
jira.NumberOfUsers=-1,
LicenseID=AAABdw0ODAoPeNp9UdtugkAQfecrSPrSPkDAC1KTTaqwNjR4iVjTJn3Z4ojbyEJmF61/XxSaar08z
pmZM+ecuRsg118Kodsd3bK6LbvbbOmeP9Mblu1oCQKIVZbngGbIYxAS6IIrnglCRzM6nUyDiGqjI
v0EHC9fJaAkhq19cWTmGTopMF4xCT5TQPb0huUYdkeriWe7HEYsBeKNh0M69YJe+Nui3znH3dFeZ
7/nZUKxWNEh42uyZSLZgki5SOxHt/2UpCVqxlmqRYAbwMAn/edmw3A773Nj4M76xmQ+fKuU5pgti
liZ+8KQ2VJtGYJZUvMNEIUFVGPXA7gQ0yU3pVChQDARX3F0Q81ZmvWd0lcY+BEdGaFrOW3LdbSyI
CfADdpIMVSAZMnWErQxJkxwyQ7+6jQ1D+EA/H/buhIwL/XsxxsnKUBpFHPksg7QBxkjzw/EL8G0p
0e1Av2++s/DR1enG7YuDrcqydc+cCnb4+PHe3+cVf0D1PsAjDAsAhRR95eLb7yzG9zInTy8vzY32
6bIagIUWrzevOp8hX3zk9snHSo/4tWRHew=X02ia,
LicenseExpiryDate=2099-12-31,
PurchaseDate=2016-06-21
```
在`System -> License`里有一个`Update License`功能可以更新License。


## 参考
* [Installing JIRA on Tomcat 6.0 or 7.0](https://confluence.atlassian.com/jira064/installing-jira-on-tomcat-6-0-or-7-0-720411825.html)
* [CentOS下安装Jira](http://allgo.cc/2015/08/18/centos%E4%B8%8B%E5%AE%89%E8%A3%85jira/)
