#### **A Simple Guide to** Connection Pooling in Java

#### 1. Overview

Connection pooling is a well-known data access pattern, whose main purpose is to reduce the overhead involved in performing database connections and read/write database operations.

In a nutshell, **a connection pool is, at the most basic level, a database connection cache implementation,** which can be configured to suit specific requirements.

#### 2. Why Connection Pooling?

The question is rhetorical, of course.

If we analyse the sequence of steps involved in a typical database connection life cycle, we'll understand why:

1. Opening a connection to the database using the database driver
2. Opening a TCP socket for reading/writing data
3. Reading/writing data over the socket
4. Closing the connection
5. Closing the socket

It becomes evident that **database connections are fairly expensive operations,** and as such, should be reduced to a minimum in every possible use case(in edge cases, just avoided).

Here's where connection pooling implementations come into play.

By just simply implementing a database connection container, which allows us to reuse a number of existing connections, we can effectively save the cost of performing a huge number of expensive database trips, hence boosting the overall performance of our database-driven applications.

#### 3.1 JDBC Connection Pooling Frameworks

### 3.1 Apache Commons DBCP

[Apache Commons DBCP Component](https://commons.apache.org/proper/commons-dbcp/download_dbcp.cgi)

#### 3.2 HikariCP

[HikariCP](https://github.com/brettwooldridge/HikariCP)

#### 3.3 C3P0

[C3PO](https://www.mchange.com/projects/c3p0/)



#### 参考来源

[A Simple Guide to Connection Pooling in Java](https://www.baeldung.com/java-connection-pooling)