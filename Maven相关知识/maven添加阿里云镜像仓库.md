#### maven 添加阿里云镜像仓库

现在的 ```java``` 项目大多都使用 **maven** 来管理我们的依赖包，默认情况下会从国外的 **maven** 中心仓库下载依赖，这样就会造成下载速度较慢的问题。 可以通过添加阿里云镜像提高 **maven** 下载依赖包的速度。

1. 打开 **maven** 安装文件的 ```settings.xml ```文件，默认情况都会在 ```C:\Users\userxx\.m2``` 下，此处 ```userxx``` 是用户名称。

   添加下面的语句：

```xml
 <mirrors>
	<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
  </mirrors>
```



2. 添加完成后的 ```settings.xml``` 文件如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <mirrors>
	<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
  </mirrors>
</settings>
```

####  Intellij IDEA 使用maven 本地仓库及修改本地仓库路径

一般 IDEA 默认了路径，所以没有所谓的setting.xml 配置。更新依赖包，只需要更改 pom.xml 配置，写好依赖包注释，就能把依赖包下载到本地仓库了。我们也可以更改本地仓库的路径，只需要一项就可以了，以下是本地仓库路径配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>


<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <localRepository>D:/maven/my_local_repository</localRepository>
  
</settings>
```



在IDEA中的设置为 File -> Setting... -> Build, Execution, Deployment -> Build Tools -> Maven 。

在其设置下有 User setting file （即 settings.xml）和 Local repository.