- 在同一对象类的内部成员变量如果被不同线程使用那一定是共享，相同的。但是如果是ThreadLocal，则当此对象类被不同线程使用时，ThreadLocal内的数据则是相互独立互补干扰的。
- ```java
  private static ThreadLocal<Integer> threadLocal = new ThreadLocal<Integer>();
  threadLocal.set();
  threadLocal.get();
  ```
- ## 原理
	- 内部维护一个哈希表，
	- key是弱引用，
	- 因为由于ThreadLocalMap的生命周期跟Thread一样长，如果都没有手动删除对应key，都会导致内存泄漏
	- value是强引用
	- 因为如果key不为空，但是value提前为空了回导致空指针异常
	- 必须要及时进行threadLocal.remove()来解决内存泄漏的问题