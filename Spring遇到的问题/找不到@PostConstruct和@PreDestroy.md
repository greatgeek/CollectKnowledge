#### 找不到@PostContruct和@PreDestroy

@PostContruct 和@PreDetroy 注解位于 java.xml.ws.annotation包是Java EE的模块的一部分。J2EE已经在Java9中被弃用，并且计划在Java 11中删除它。

#### 解决办法

为pom.xml 或 build.gradle 添加必要的依赖项

（Java 9+ 中的Spring @PostConstruct 和 @PreDestroy 替代品）

```xml
        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>javax.annotation-api</artifactId>
            <version>1.3.2</version>
        </dependency>
```

添加后即可解决找不到@PostContruct 和 @PreDestroy的问题。