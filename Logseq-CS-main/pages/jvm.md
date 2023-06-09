- ## [[内存结构]]
- ## [[类加载]]
- ## [[垃圾回收机制]]
- ## [[四大引用]]
- ## 内存模型JMM
	- 所有变量都存储在主内存中，每个线程都有自己的工作内存，工作内存中存有主内存的拷贝
	- 线程间进行通信：线程A把本地内存更新到主内存，线程B从主线程中读取线程A的数据
- ## 直接内存
	- 直接内存不受jvm虚拟机管理，是通过java中的unsafe对象分配一块直接内存
	- ### 作用
		- 访问直接内存的速度会优于java堆。即读写性能高，出于性能考虑，读写频繁的场合可能会考虑使用直接内存。
- ## 面试
	- ### 为什么说JVM是跨平台的？
		- 因为Java程序编译之后的代码不是能被硬件系统直接运行的代码，而是一种“中间码”——字节码。然后不同的硬件平台上安装有不同的Java虚拟机(JVM)，由JVM来把字节码再“翻译”成所对应的硬件平台能够执行的代码。
	- ### 比较两个类是否相同？
		- 类的文件路径一致并且是同一类加载器加载出来的
	- ### 垃圾回收机制触发条件和调用 System.gc()的区别？
		- 垃圾回收触发条件：当前应用线程没有在运行时GC会被调用。或堆内存不够时会被触发
		- System.gc()相当于给虚拟机发送一个提醒信号，希望他进行垃圾回收
	- ### 如何改善GC垃圾回收
		- 对象不用后设置为null，必要的时候使用其他三大引用
		- 尽量减少静态变量，临时变量使用
		- 尽量使用StringBuffer来累加字符串而不是使用多个“+”
	- ### 堆中的对象如何由新生代进入老年代？
		- 对象在s0,s1区每熬过一次Minor GC，就将对象的年龄加1，当对象的年龄到达某个值时(默认是15，可以通过参数-XX:MaxTenuringThreshold来设定),就会进入老年代
		- 如果对象创建是本身就需要很大的连续内存也会直接进入老年代
	- ### 对象不可达，一定会被垃圾收集器回收么？
		- 即使不可达，对象也不一定会被垃圾收集器回收，1）先判断对象是否有必要执行finalize()方法，对象必须重写finalize()方法且没有被运行过。2）若有必要执行，会把对象放到一个队列中，JVM会开一个线程去回收它们，这是对象最后一次可以逃逸清理的机会。