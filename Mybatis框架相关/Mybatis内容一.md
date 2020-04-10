#### Mybatis 内容一

#### 1.JDBC 编程分析

#### 1.1 JDBC 程序步骤回顾

1. 加载数据库驱动

   ```java
   Class.forName("com.mysql.jdbc.Driver");
   ```

2. 通过驱动管理类获取数据库连接

   ```java
   connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/eesy","root", "1234");
   ```

3. 获取预处理 statement （PreparedStatement）

   ```java
   String sql = "select * from user where username = ?"; // 定义sql 语句，？表示占位符
   preparedStatement = connection.prepareStatement(sql); // 获取预处理 statement
   preparedStatement.setString(1,"王五"); // 设置参数，第一个参数为sql 语句中参数的序号（从1开始），第二个参数为设置的参数值
   ```

4. 向数据库发出sql 执行查询，查询出结果集

   ```java
   resultSet = preparedStatement.executeQuery();
   ```

5. 处理结果集(遍历或是其他处理方式)

   ```java
   //遍历查询结果集
   while(resultSet.next()){
   System.out.println(resultSet.getString("id")+"
   "+resultSet.getString("username"));
   }
   ```

6. 释放资源

   ```java
   resultSet.close();
   preparedStatement.close();
   connection.close();
   ```

   

#### 1.2 jdbc 问题分析

1. 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库连接池可解决此问题。
2. sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能性较大，sql 变动需要改变 java 代码。
3.  使用 preparedStatement 向占位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
4. 对结果集解析存在硬编码（查询列名），sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成 POJO对象解析比较方便。

**Note:**

持久层技术解决方案：

1. JDBC 技术

   Connection

   PreparedStatement

   ResultSet

2. Spring的JdbcTemplate:

   Spring中对jdbc 的简单封装

3. Apache的DBUtils:

   它和Spring的JdbcTemplate很像，也是对Jdbc的简单封装

以上这些都不是框架

* JDBC是规范
* Spring的JdbcTemplate和Apache的DBUtils都只是工具类。

#### 2.Mybatis 框架

#### 2.1 Mybatis 环境搭建

1. 创建Maven工程并导入坐标；
2. 创建实体类和dao 的接口：IUserDao.java；（在java 目录下）
3. 创建Mybatis 的主配置文件：SqlMapConfig.xml；（在resource 目录下）
4. 创建映射配置文件：IUserDao.xml

环境搭建的注意事项：

1. 创建IUserDao.xml 和IUserDao.java 时名称只是为了保持一致，在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper。所以IUserDao 和 IUserMapper是一样的。
2. 在IDEA中创建目录时，目录和包是不一样的。包在创建时：com.itheima.dao是三级目录。目录在创建时：com.itheima.dao是一级目录，而 com/itheima/dao是三级目录。
3. Mybatis的映射配置文件位置必须和dao 接口的包结构相同。
4. 映射配置文件的mapper标签namespace属性的取值必须是dao 接口的全限定类名。
5. 映射配置文件的操作配置（select ），id 属性的取值必须是 dao 接口的方法名。

当我们遵从了第三、四、五点后，我们在开发中就无须再写dao 的实现类。

#### 3.Mybatis 入门

来源于：[Mybatis 官网](https://mybatis.org/mybatis-3/zh/getting-started.html)

#### 3.1 安装

要使用Mybatis，只需要将mybatis-x.x.x.jar 文件置于类路径(classpath)中即可。

如果使用Maven来构建项目，则需要将依赖置于pom.xml 文件中：

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

#### 3.2 从XML中构建SqlSessionFactory

每个基于MyBatis 的应用都是以一个 **SqlSessionFactory** 的实例为核心的。**SqlSession**的实例可以通过 **SqlSessionFactoryBuilder** 获得。而**SqlSessionFactoryBuilder**则可以从**XML**配置文件或一个预配置的**Configuration** 实例来构建出**SqlSessionFactory** 实例。

从XML文件中构建SqlSessionFactory 的实例非常简单，建议使用类路径下的资源文件进行配置。但也可以使用任意的输入流（InputStream）实例，比如用文件路径字符串或 fill:// URL 构造的输入流。MyBatis 包含一个名叫 Resources的工具类，它包含一些实用的方法，使得从类路径或其它位置加载资源更加容易。

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

XML 配置文件中包含了对MyBatis系统的核心设置，包括获取数据库连接实例的数据源（DataSource）以及决定事务作用域和控制方式的事务管理器（TransactionManager）。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

当然，还有很多可以在XML文件中配置的选项，上面的示例仅罗列了最关键的部分。注意XML头部的声明，它用来验证XML文档的正确性。environment 元素体中包含了事务管理和连接池的配置。mappers 元素则包含了一组映射器（mapper），这些映射器的XML映射文件包含了SQL代码和映射定义信息。

同样提供了不使用XML构建SqlSessionFactory的方法。

#### 3.3 从SqlSessionFactory 中获取 SqlSession

既然有了SqlSessionFactory，顾名思义，我们可以从中获得SqlSession 的实例。SqlSession 提供了在数据库执行SQL命令所需要的所有方法。你可以通过SqlSession 实例来直接执行已映射的SQL语句。例如：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
}
```

诚然，这种方式能够正常工作，对使用旧版本MyBatis的用户来说也比较熟悉。但现在有了一种更简洁的方式——使用和指定语句的参数和返回值相匹配的接口（比如BlogMapper.class），现在你的代码不仅更清晰，更加类型安全，还不用担心可能出错的字符串字面值以及强制类型转换。

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
}
```

现在我们来探究一下这段代码究竟做了什么。

#### 3.4 探究已映射的SQL语句

在上面提到的例子中，一个语句即可以通过XML定义，也可以通过注解定义。这里给出一个基于XML映射语句的示例，它应该可以满足上个示例中SqlSession的调用。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

在一个XML映射文件中，可以定义无数个映射语句，这样一来，XML头部和文档类型声明部分就显得微不足道了。文档的其他部分很直白，容易理解。它在命名空间“org.mybatis.example.BlogMapper”中定义了一个名为“selectBlog”的映射语句，这样你就可以用全限定名“org.mybatis.example.BlogMapper.selectBlog”来调用映射语句了，就像上面例子中那样：

```java
Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
```

你可以会注意到，这种方式和用全限定名调用Java对象的方法类似。这样，该命令就可以直接映射到在命名空间的映射器类，并将已映射的 select 语句匹配到对应名称、参数和返回类型的方法。因此你就可以像上面那样，不费吹灰之力地在对应的映射器接口调用方法，就像下面这样：

```java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```

第二种方法有很多优势，首先它不依赖于字符串字面值，会更安全一点；其次，如果你的IDE有代码补全功能，那么代码补全可以帮你快速选择到映射好的SQL语句。

**Tips：对命名空间的一点补充**

在之前版本的MyBatis中，命名空间（Namespaces）的作用并不大，是可选的。但现在，随着命名空间越发重要，你必须指定命名空间。

命名空间的作用有两个，一个是利用更长的全限定名来将不同的语句隔离开来，同时也实现了你上面见到的接口绑定。就算你觉得暂时用不到接口绑定，你也应用遵循这里的规定，以防哪天你改变了主意。长远来看，只要将命名空间置于合适的Java 包命名空间之中，你的代码会变得更加整洁，也有利于更方便地使用MyBatis。

**命名解析：**为了减少输入量，MyBatis对所有具有名称的配置元素（包括语句，结果映射，缓存等）使用了如下的命名解析规则 。

* 全限定名将被直接用于查找及使用。
* 短名称（比如“selectAllThings”）如果全局唯一也可以作为一个单独的引用。如果不唯一，有两个或两个以上的相同名称（比如“com.foo.selectAllThings”和“com.bar.selectAllThings”），那么使用时就会产生“短名称不唯一”的错误，这种情况下就必须使用全限定名。

---

对于像BlogMapper这样的映射器类来说，还有另一种方法来完成语句。它们映射的语句可以不用XML来配置，而可以使用Java注解来配置。比如，上面的XML示例可以被替换成如下的配置：

```java
package org.mybatis.example;
public interface BlogMapper {
  @Select("SELECT * FROM blog WHERE id = #{id}")
  Blog selectBlog(int id);
}
```

使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java注解不仅力不从心，还会让你本就复杂的SQL语句更加混乱不堪。因此，如果你需要做一些很复杂的操作，最好用XML来映射语句。

选择何种方式来配置映射，以及认为是否应该要统一映射语句定义的形式，完全取决于你和你的团队。换句话说，永远不要拘泥于一种方式，你可以很轻松的在基于注解和 XML 的语句映射方式间自由移植和切换。      

#### 3.5 作用域（Scope）和生命周期

理解我们之前讨论过的不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的并发问题。

---

**Tips:对象生命周期和依赖注入框架**

依赖注入框架可以创建线程安全的、基于事务的SqlSession 和映射器，并将它们直接注入到你的 bean 中，因此可以直接忽略它们的生命周期。

---

**SqlSessionFactoryBuilder**

这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory ，就不再需要它了。因此 SqlSessionFactoryBuilder实例的最佳作用域是**方法作用域**（也就是局部方法变量）。你可以重用SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory实例，但最好还是不要一直保留着它，以保证所有的XML解析资源可以被释放给更重要的事情。

**SqlSessionFactory**

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。使用 SqlSessionFactory的最佳实践是在应用运行期间不要重复创建多次，多次重建SqlSessionFactory 被视为一种代码“坏习惯”。因此SqlSessionFactory的最佳作用域是应用使用域。有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

SqlSession

每个线程都应该有它自己的SqlSession实例。SqlSession的实例不是线程安全的，因此是不能被共享的，所以它的最佳作用域是请求或方法作用域。绝对不能将SqlSession实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。也约不能将SqlSession实例的引用入在任何类型的托管作用域中，比如Servlet 框架中的HttpSession。如果你现在正在使用一种Web框架，考虑将SqlSession 放在一个和HTTP 请求相似的作用域中。换句话说，每次收到HTTP请求，就可以打开一个SqlSession，返回一个响应后，就关闭它。这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到finally块中。下面的示例就是一个确保SqlSession关闭的标准模式：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  // 你的应用逻辑代码
}
```

在所有代码中都遵循这种使用模式，可以保证所有数据库资源都能被正确地关闭。

#### 3.5映射器实例

映射器是一些绑定映射语句的接口。映射器接口的实例是从SqlSession中获得的。虽然从技术层面上来讲，任何映射器实例的最大作用域与请求它们的SqlSession相同。但方法作用域才是映射实例的最合适的作用域。也就是说，映射器实例应该在调用它们的方法中被获取，使用完毕之后即可丢弃。映射器实例并不需要被显式地关闭。尽管在整个请求作用域保留映射器实例不会有什么问题，但是你很快会发现，在这个作用域上管理太多像SqlSession的资源会让你忙不过来。因此，最好将映射器放在方法作用域内。就像下面的例子一样：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  // 你的应用逻辑代码
}
```

