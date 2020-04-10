#### Commons DbUtils: JDBC Utility Component

The Commons DbUtils library is a small set of classes designed to make working with JDBC easier. JDBC resource cleanup code is mundane, error prone work so these classes abstract out all of the cleanup tasks from your code leaving you with what you really wanted to do with JDBC in the first place: query and update data.

Some of the advantages of using DbUtils are:

* No possibility for resource leaks. Correct JDBC coding isn't difficult but it is time-consuming and tedious. This often leads to connection leaks that may be difficult to track down.
* Cleaner, clearer persistence code. The amount of code needed to persist data in a database is drastically reduced. The remaining code clearly expresses your intention without being cluttered with resource cleanup.
* Automatically populate JavaBean properties from ResultSets. You don't need to manually copy column values into bean instances by calling setter methods. Each row of the ResultSet can be represented by one fully populated bean instance.



#### 参考来源

[Commons DbUtils: JDBC Utility Component](https://commons.apache.org/proper/commons-dbutils/)