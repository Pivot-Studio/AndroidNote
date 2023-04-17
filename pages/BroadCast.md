- 比如电池的使用状态，电话的接收和短信的接收都会产生一个广播，应用程序开发者也可以监听这些广播并做出程序逻辑的处理。这些操作可以是跨进程的也可以是在本进程内的。
- ## 原理
	- 广播接收者BroadcastReceiver通过Binder机制向AMS(Activity Manager Service)进行注册
	- 广播发送者通过binder机制向AMS发送广播
	- AMS查找符合相应条件（IntentFilter/Permission等）的BroadcastReceiver，将广播发送到BroadcastReceiver相应的消息循环队列中
	- 消息循环队列拿到此广播后，回调BroadcastReceiver中的onReceive()方法
- ## 使用
	- ### 广播的发送
		- ```java
		  Intent intent=new Intent();
		  intent.setAction(BROADCAST_ACTION_NORMAL)
		  sendBroadcast(intent)
		  ```
	- ### 广播的接受
		- 在androidmanifest.xml中申请权限
		- #### 动态注册（但是必须要在程序启动后才可以监听）
			- 创建intentFilter类型变量为其添加action等（相当于静态注册中的AndroidManifest.xml）
			- 在使用时registerReceiver方法传递的两个实参分别为自定义广播类和intentFilter
			- 在ondestroy方法中调用unregisterReceiver方法传递的实参为自定义广播类
		- #### 静态注册（随时都可以监听）
			- 直接在Androidmanifest.xml中进行相应操作即可。
		- ### onReceive方法
			- onReceive方法中不能执行太耗时的操作，否则将引起ANR,
- ## 面试
	- ### 广播如何实现本地广播？
		- 在普通广播的基础上(主要针对动态注册)，一般发送接受广播所调用的方法
		- sendBroadcast,registerReceiver,unregisterReceiver都是基于context的,这里创建LocalBroadCastManager变量，然后调用的这些方法就可以实现本地广播
	- ### 广播如何实现有序广播？
		- 无论是静态注册还是动态注册为其intentfilter添加一条priority属性值，数越大级别越高，范围是-1000到1000
-