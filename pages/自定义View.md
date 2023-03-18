- ## View移动
	- 属性动画
	- scrollTo
	- scrollBy
	- scroller+computeScroll
- ## View绘制
	- View随着Activity的创建而加载，startActivity启动一个Activity时，在ActivityThread的handleLaunchActivity方法中会执行Activity的onCreate方法，这个时候会调用setContentView加载布局创建出DecorView并将我们的layout加载到DecorView中，当执行到handleResumeActivity时，Activity的onResume方法被调用，然后WindowManager会将DecorView设置给ViewRootImpl,这样，DecorView就被加载到Window中了，此时界面还没有显示出来，还需要经过View的measure，layout和draw方法，才能完成View的工作流程。
	- 大致流程
		- View的绘制过程遵循如下几步：
		  a.绘制背景 background.draw(canvas)
		  b.绘制自己（onDraw）
		  c.绘制children（dispatchDraw）
		  d.绘制装饰（onDrawScrollBars）
- ## View动画
- ## 面试
	- ### 子view宽高可以超过父view？
		- 能，android:clipChildren = "false" 这个属性要设置在父 view 上。代表其中的子View 可以超出屏幕。
	- ### 自定义View需要注意哪些东西？
		- 1.让view支持wrap_content属性，在onMeasure方法中针对AT_MOST模式做专门处理，否则wrap_content会和match_parent效果一样（继承ViewGroup也同样要在onMeasure中做这个判断处理）
		- if(widthMeasureSpec == MeasureSpec.AT_MOST && heightMeasureSpec == MeasureSpec.AT_MOST){
		- setMeasuredDimension(200,200);
		- 2.view中有线程或者动画 要及时停止，