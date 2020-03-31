#### IOC的概念和作用

控制反转（Inversion of Control, 英文缩写为 IOC）把创建对象的权利交给框架，是框架的重要特征，并非面向对象编程的专用术语。它包括依赖注入（Dependency Injection，简称DI）和依赖查找（Dependency Lookup）。

#### 1. 解耦的思路

1. 使用反射来创建对象，而避免使用 new 关键字。
2. 通过读取配置文件来获取要创建的对象的全限定类名。

#### 2. 用工厂模式来理解 IOC

### 2.1 [Java Property File ](https://www.w3resource.com/java-tutorial/java-propertyfile-processing.php)

**Introduction**

Properties is a file extension for files mainly used in Java related technologies to store the configurable parameters of an application. Java Properties files are amazing resources to add information in Java. Generally, these files are used to store static information in **key and value pair**. Things that you do not want to hard code in your Java code goes into properties files. The advantage of using properties file is we can configure things which are prone to change over a period of time without the need of changing anything in code. Properties file provide flexibility in terms of configuration. Sample properties file is shown below, which has information in key-value pair.

Each parameter is stored as a pair of strings, left side of equal(=) sign is for storing the name of the parameter (called the key), and the other storing the value.

**Configuration:**

```properties
# This is property file having my configuration
FileName=Data.txt
Author_Name=Amit Himani
Website=w3resource.com
Topic=Properties file Processing 

```

First line which starts with # is called comment line. We can add comments in properties which will be ignored by Java compiler.

Below is the Java program for reading above property file:

```java
package propertiesfile;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;
public class PropertyFileReading {
	public static void main(String[] args) {
		Properties prop = new Properties();
		try {
			// load a properties file for reading
			prop.load(new FileInputStream("myConfig.properties"));
			// get the properties and print
			prop.list(System.out);
			//Reading each property value
			System.out.println(prop.getProperty("FileName"));
			System.out.println(prop.getProperty("Author_Name"));
			System.out.println(prop.getProperty("Website"));
			System.out.println(prop.getProperty("TOPIC"));
		} catch (IOException ex) {
			ex.printStackTrace();
		}
	}
}

```



### 2.2 [Java.util.Properties class in Java](https://www.geeksforgeeks.org/java-util-properties-class-java/)

The Properties class represents a persistent set of properties. The Properties can be saved to a stream or loaded from a stream.

* **Properties** is a subclass of **Hashtable**.
* It is used to maintain list of value in which the key is a string and the value is also a string.
* One useful capability of the Properties class is that you can specify a default property that will be returned if no value is associated with a certain key.
* Multiple thread can share a single properties object without the need external synchronization.

### 2.3 使用工厂模式来实现IOC的思路

1. 实现需要的类
2. 将类的id 与 全限定类名写入. properties 文件
3. 在工厂模式中通过读取 .properties 文件获取 全限定类名的方式来反射创建类的实例.

#### 3. 使用Spring 的 IOC

**使用 Maven 来管理包依赖关系**

1. 实现需要的类
2. 创建bean.xml, 并将类的id 与全限定类名写入bean.xml文件
3. 传入bean.xml 来生成Spring的核心容器
4. 根据id 生成对象

#### 4. Notes

#### 4.1 ApplicationContext的三个常用实现类

1. ```ClassPathXmlApplicationContext```: 它可以加载类路径下的配置文件,要求配置文件必须在类路径下.不在的话,加载不了(**更常用**)
2. ```FileSystemXmlApplicationContext```: 它可以加载磁盘任意路径下的配置文件(必须要有访问权限)
3. ```AnnotationConfigApplication```:它用于读取注解创建容器

#### 4.2 核心容器的两个接口引发出的问题

1. ``` ApplicationContext```: 单例对象适用		采用此接口

   它在构建核心容器时,创建对象采取的策略是**立即加载**的方式. 也就是说,只要读取完配置文件马上就创建配置文件中配置的对象;

2. ``` BeanFactory```: 多例对象使用  

   它在构建核心容器时,创建对象的策略是采用**延迟加载**的方式. 也就是说,什么时候根据 id 获取对象了,什么时候才真正的创建对象.



##### 5. Bean对象

把对象的创建交给Spring 来管理,Spring 对 bean 的管理细节:

1. 创建bean的三种方式
2. bean对象的作用范围
3. bean对象的生命周期

### 5.1 创建Bean的三种方式

第一种方式：使用默认构造函数创建。

在spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时。采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建。

```xml
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
```

第二种方式： 使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入spring容器

```xml
<bean id="instanceFactory" class="com.itheima.factory.InstanceFactory"></bean>
<bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
```

第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入spring容器)

```xml
<bean id="accountService" class="com.itheima.factory.StaticFactory" factory-method="getAccountService"></bean>
```

#### 5.2 bean的作用范围调整

bean标签的scope属性：
            作用：用于指定bean的作用范围
            取值： 常用的就是单例的和多例的

* singleton：单例的（默认值）
* prototype：多例的
* request：作用于web应用的请求范围
* session：作用于web应用的会话范围
* global-session：作用于集群环境的会话范围（全局会话范围），当不是集群环境时，它就是session

### 5.3 bean 对象的生命周期

单例对象
                出生：当容器创建时对象出生
                活着：只要容器还在，对象一直活着
                死亡：容器销毁，对象消亡
                总结：**单例对象的生命周期和容器相同**
多例对象
                出生：当我们使用对象时spring框架为我们创建
                活着：对象只要是在使用过程中就一直活着。
                死亡：**当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收**

#### 6. 依赖注入

依赖关系的管理交给spring 来维护,在当前类需要用到其他类的对象,由spring 为我们提供,我们只需要在配置文件中说明,依赖关系的维护就称之为依赖注入.

依赖注入：
            能注入的数据：有三类
                基本类型和String
                其他bean类型（在配置文件中或者注解配置过的bean）
                复杂类型/集合类型
             注入的方式：有三种
                第一种：使用构造函数提供
                第二种：使用set方法提供
                第三种：使用注解提供(在注解的部分会讲到)

### 6.1 构造函数注入

使用的标签:constructor-arg
        标签出现的位置：bean标签的内部
        标签中的属性
            type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
            index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引的位置是从0开始
            name：用于指定给构造函数中指定名称的参数赋值                                        常用的
            =============以上三个用于指定给构造函数中哪个参数赋值===============================
            value：用于提供基本类型和String类型的数据
            ref：用于指定其他的bean类型数据。它指的就是在spring的Ioc核心容器中出现过的bean对象

   优势：
        在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
   弊端：
        改变了bean对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供。

### 6.2 set 方法注入(更为常用的方式)

涉及的标签：property
        出现的位置：bean标签的内部
        标签的属性
            name：用于指定注入时所调用的set方法名称
            value：用于提供基本类型和String类型的数据
            ref：用于指定其他的bean类型数据。它指的就是在spring的Ioc核心容器中出现过的bean对象
        优势：
            创建对象时没有明确的限制，可以直接使用默认构造函数
        弊端：
            如果有某个成员必须有值，则获取对象是有可能set方法没有执行。

### 6.2.1 复杂类型的注入/集合类型的注入

​		用于给List结构集合注入的标签：
​            list array set
​        用于个Map结构集合注入的标签:
​            map  props
​        结构相同，标签可以互换

### 6.3 使用注解注入

在注解的部分会讲到

#### 参考来源

[Java Property File Processing](https://www.w3resource.com/java-tutorial/java-propertyfile-processing.php)

 [Java.util.Properties class in Java](https://www.geeksforgeeks.org/java-util-properties-class-java/)

