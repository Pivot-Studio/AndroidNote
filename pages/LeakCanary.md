- ## 使用
	- 导入依赖后使用LeakCanary.install(this)初始化即可
-
- ## `LeakCanary`的原理是什么？（针对Activity来说）
	- `LeakCanary` 通过监听Activity生命周期，在Activity onDestroy的时候，创建一个弱引用，并且关联一个引用队列，并创建一个key和当前Activity绑定，将key保存到set数据结构里面，然后在主线程空闲5秒后，开始检测是否内存泄漏，具体检测步骤：
	- 1：判断引用队列中是否有该Activity的引用，有则说明Activity被回收了，移除Set里面对应的key。
	- 2：判断Set里面是否有当前要检测的Activity的key，如果没有，说明Activity对象已经被回收了，没有内存泄漏。如果有，只能说明Activity对象还没有被回收，可能此时已经没有被引用，不一定是内存泄漏。
	- 3：手动触发GC，然后重复1和2操作，确定一下是不是真的内存泄漏。