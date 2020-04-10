#### 1. 基于代理Dao 实现CRUD 操作

使用要求：

1. 持久层接口和持久层接口的映射配置文件必须在相同包下；
2. 持久层映射配置中 mapper 标签的 namespace 属性取值必须是持久层接口的全限定类名；
3. SQL语句的配置标签```<slect>, <insert>, <delete>, <update>```的id 属性必须和持久层接口的方法名相同。

#### 1.1 XML 映射器

SQL 映射文件只有很少的几个顶级元素（按照应定义的顺序列出）。

* ```cache``` -该命名空间的缓存配置。
* ``` cache-ref``` -引用其它命名空间的缓存配置。
* ``` resultMap```-描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素
* ``` sql```-可被其他语句引用的可重用语句块。
* ``` insert```- 映射插入语句
* ``` update```- 映射更新语句
* ``` delete```- 映射删除语句
* ``` select``` - 映射查询语句

#### 1.1.1 select 

一个简单查询的select 元素是非常简单的。比如：

```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

这个语句名为selectPerson， 接受一个 int （或Integer）类型的参数，并返回一个hashmap类型的对象，其中的键是列名，值便是结果行中的对应值。

注意参数符号：

```xml
#{id}
```

这就告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC          中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中，就像这样：

```java
// 近似的 JDBC 代码，非 MyBatis 代码...
String selectPerson = "SELECT * FROM PERSON WHERE ID=?";
PreparedStatement ps = conn.prepareStatement(selectPerson);
ps.setInt(1,id);
```

更多内容需要参考：[XML映射器](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#Result_Maps)

#### 1.2 XML配置

MyBatis的配置文件包含了会深深影响MyBatis行为的设置和属性信息。配置文件档的顶层结构如下 ：

* ```configuration```（配置）
  * ```properties```（属性）
  * ```settings```（设置）
  * ```typeAliases```（类型别名）
  * ```typeHandlers```（类型处理器）
  * ```objectFactory```（对象工厂）
  * ```plugins```（插件）
  * ```environments```（环境配置）
    * ```environment```（环境变量）
      * ```transactionManager```（事务管理器）
      * ```dataSource```（数据源）
  * ```databasesIdProvider```（数据库厂商标识）
  * ```mappers```（映射器）

更多内容需要参考MyBatis官方文档。

#### 参考来源

[XML映射器](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#Result_Maps)