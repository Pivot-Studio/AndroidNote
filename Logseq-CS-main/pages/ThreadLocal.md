- 在同一对象类的内部成员变量如果被不同线程使用那一定是共享，相同的。但是如果是ThreadLocal，则当此对象类被不同线程使用时，ThreadLocal内的数据则是相互独立互补干扰的。
- ```java
  private static ThreadLocal<Integer> threadLocal = new ThreadLocal<Integer>();
  threadLocal.set();
  threadLocal.get();
  ```