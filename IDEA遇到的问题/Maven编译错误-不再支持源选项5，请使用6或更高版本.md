#### 问题描述：Error: java: 不再支持源选项5。请使用6或更高版本

1. 修改maven setting.xml 中的数据（全局修改）

   

   ```xml
   	<profile>
   		<id>jdk-11</id>
   		<activation>
   			<activeByDefault>true</activeByDefault>
   			<jdk>11</jdk>
   		</activation>
   		
   		<properties>
   			<maven.compiler.source>11</maven.compiler.source>
   			<maven.compiler.target>11</maven.compiler.target>
   			<maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
   		</properties>
   	</profile>
   ```

   这里使用的java 版本要对应现在使用的jdk 版本。

2. 为单独的项目制定jdk 版本（局部修改）

   ```xml
   		<properties>
   			<maven.compiler.source>11</maven.compiler.source>
   			<maven.compiler.target>11</maven.compiler.target>
   			<maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
   		</properties>
   ```

   

3. 检查 IDEA 配置

* 项目配置检查

File -> Project Structure.... -> Project SDK 和 Project language level 。这两块地方的版本要一致

​											-> Sources / Language level 。这个版本也要一致

![Project](I:\GreatGeek\相关知识\IDEA遇到的问题\Maven编译错误-不再支持源选项5，请使用6或更高版本.assets\image-20200318175437842.png)

![Modules](I:\GreatGeek\相关知识\IDEA遇到的问题\Maven编译错误-不再支持源选项5，请使用6或更高版本.assets\image-20200318175505069.png)

* 全局配置检查

File -> settings... -> Build, Exceution, Deployment -> Compiler -> Java Compiler / Project bytecode version 这里也要一致

![image-20200318175619593](I:\GreatGeek\相关知识\IDEA遇到的问题\Maven编译错误-不再支持源选项5，请使用6或更高版本.assets\image-20200318175619593.png)