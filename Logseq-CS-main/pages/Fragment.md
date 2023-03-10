- ## 注册
	- ### 静态注册
		- 在布局中表明fragment的具体类地址，activity会自动加载
	- ### 动态注册
		- 获取fragmentmanager对象
		- 调用fragmentmanager对象的begintransaction()获取到FragmentTranslation对象
		- 调用FragmentTranslation对象的replace方法，传入替换fragment的id,替换的fragment对象，之后执行commit即可。
- ## 数据传递
	- ### fragment与activity之间进行数据传递
		- MainActivity activity = (MainActivity) getActivity();
	- ### fragment与fragment之间进行数据传递
		- ```java
		  MainFragment mainFragment =
		               (MainFragment) getActivity()
		               .getSupportFragmentManager()
		               .findFragmentByTag("mainFragment");
		  ```
		- 内部匿名类实现监听接口
- ## 使用
	- 类似微信下方菜单栏
	- 一个界面中只有某一确定位置需要多次改动，其他部分可以复用的情况
	- 以及手机和平板适配