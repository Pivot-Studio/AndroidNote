- 实现Callable接口，内部方法为call,有返回值可以确定是完成的是哪一个线程,需要通过FutureTask封装Callable,再传入Thread()
- ```java
  FutureTask<Integer> ft = new FutureTask<Integer>(new MyCallable());
  new Thread(ft).start();
  ```