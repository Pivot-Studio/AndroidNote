- AIDL是Android Interface Definition Language的缩写，即Android接口定义语言。它是Android的进程间通信比较常用的一种方式。
- ## 注意
	- 在Java中，如果一个对象和引用它的类在同一个package下，是不需要导包的，即不需要import，而在AIDL中，自定义的Parceable对象和AIDL接口定义的对象必须在所引用的AIDL文件中显式import进来，不管这些对象和所引用它们的AIDL文件是否在同一个包下。
- ## 使用
	- 客户端开发步骤：
	  1.将服务端的AIDL文件夹拷贝至客户端的main文件夹下
	  2.rebuild project,检查是否生成对应的Java文件
	  3.客户端的activity里连接远程服务,实现aidl通信