#### HashMap 与HashTable的区别。

1. HashTable的方法是同步的，而HashMap更加纯粹些并没有实现同步，所以在多线程场合下要手动同步HashMap。这个区别就像是Vector和ArrayList一样。
2. HashTable不允许null值（Key 和 Value都不可以），HashMap允许null值 （Key值和Value都可以）。

#### HashMap中的Key可以是任何对象或数据类型么？

* 可以为null ，但不能是可变对象，如果是可变对象的话，对象中的属性改变，则对象HashCode也会相应的改变，导致下次无法查找到Key。

  **设计实验说明：**

  ```java
      public static void main(String[] args) {
          List<String> list = new LinkedList<>();
          list.add("A");
          HashMap<List<String>,Integer> map = new HashMap<>();
          map.put(list,1);
          list.add("B"); // 再次添加"B",会导致list对象的属性发生改变，会重新计算HashCode
          int res = map.get(list);// 此时无法找到这个Key，因为变化后的HashCode从未存入过HashMap,这句会执行出错
          System.out.println(res);
      }
  ```

* 如果可变对象在HashMap中被用作Key，那就要小心在改变对象的状态时，不要改变它的哈希值了。我们只需要保证成员变量的改变能保证该对象的哈希值不变即可。

  设计实验说明：

  ```java
  static class MyList extends LinkedList<String>{
          public String name;// 用 name 来判断两个对象是否相等
  
           public MyList(String name){
               super();
               this.name=name;
           }
           public String getName() {
               return name;
           }
  
           @Override
           public boolean equals(Object obj){ // 此处重写equals方法并不是必要的，只是习惯写法（通常重写euqlas 也要重写hasCode）
               if(this == obj){
                   return true;
               }
  
               if(obj==null){
                   return false;
               }
  
               if(obj instanceof MyList){
                   MyList other = (MyList)obj;
                   if(other.getName().equals(this.getName())){
                       return true;
                   }
               }
               return false;
           }
  
           @Override
           public int hashCode(){ // 重写的hashCode() 方法，则仅当 name 属性改变时，hashCode才会改变
               int result = 17;
               result =31*result +(name==null ? 0 : name.hashCode());
               return result;
           }
      }
  
      public static void main(String[] args) {
          MyList list = new MyList("myList");
          list.add("A");
          HashMap<List<String>,Integer> map = new HashMap<>();
          map.put(list,1);
          list.add("B");// 此时添加“B”不会导致对象发生变化
          int res = map.get(list); // 可c正常获取Value
          System.out.println(res);
      }
  ```

  

