- ## View移动
	- 属性动画
	- scrollTo
	- scrollBy
	- scroller+computeScroll
- ## View绘制
	- 绘制过程
		- View随着Activity的创建而加载，startActivity启动一个Activity时，在ActivityThread的handleLaunchActivity方法中会执行Activity的onCreate方法，这个时候会调用setContentView加载布局创建出DecorView并将我们的layout加载到DecorView中，当执行到handleResumeActivity时，Activity的onResume方法被调用，然后WindowManager会将DecorView设置给ViewRootImpl,这样，DecorView就被加载到Window中了，此时界面还没有显示出来，还需要经过View的measure，layout和draw方法，才能完成View的工作流程。
	- 绘制图层顺序
		- View的绘制过程遵循如下几步：
		  a.绘制背景 background.draw(canvas)
		  b.绘制自己（onDraw）
		  c.绘制children（dispatchDraw）
		  d.绘制装饰（onDrawScrollBars）
	- 绘制原理
		- Android的屏幕刷新中涉及到最重要的三个概念（为便于理解，这里先做简单介绍）
		- CPU：执行应用层的measure、layout、draw等操作，绘制完成后将数据提交给GPU
		- GPU：进一步处理数据，并将数据缓存起来
		- 屏幕：由一个个像素点组成，以固定的频率（16.6ms，即1秒60帧）从缓冲区中取出数据来填充像素点
- ## View动画
	- 动画分为帧动画，view动画，属性动画
	- ### 帧动画
		- 就和电视一样通过加载Drawable资源将逐个逐个图片进行拼接
		- 可以设置动画是否重复播放以及每个图片的持续时间
	- ### view动画（补间动画）
		- 由移动，缩放，透明度，旋转组成四种类型的补间动画；并且View动画还提供了动画集合类（AnimationSet），通过动画集合类（AnimationSet）可以将多个补间动画以组合的形式显示出来。
		- animationSet还可以实现监听，监听动画开始，动画取消，动画结束，动画重复执行
		- 但是补间动画只是实现了视觉效果上做了移动、缩放、旋转和淡入淡出的效果，其实并没有真正改变 View 的属性。
	- ### 属性动画
		- 相当于是view动画的增强版，除了view动画的四种基本方式，还有有其他复杂的动画可以供直接调用
		- 属性动画不光实现了视觉上的移动，其点击事件位置也跟随发生变化。
- ## 面试
	- ### 子view宽高可以超过父view？
		- 能，android:clipChildren = "false" 这个属性要设置在父 view 上。代表其中的子View 可以超出屏幕。
	- ### 自定义View需要注意哪些东西？
		- 1.让view支持wrap_content属性，在onMeasure方法中针对AT_MOST模式做专门处理，否则wrap_content会和match_parent效果一样（继承ViewGroup也同样要在onMeasure中做这个判断处理）
		- if(widthMeasureSpec == MeasureSpec.AT_MOST && heightMeasureSpec == MeasureSpec.AT_MOST){
		- setMeasuredDimension(200,200);
		- 2.view中有线程或者动画 要及时停止，避免内存泄漏