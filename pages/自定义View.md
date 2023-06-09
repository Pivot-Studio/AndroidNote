- ## 自定义View类型
	- 继承自组合控件：这周不需要重写onmeasure,onlayout,ondraw方法，是将安卓本身提供的各种类型view根据自己需求封装到一个自定义view类里面，在其内部通过LayoutInflate.inflate进行布局绑定，直接当做正常控件进行使用，主要是为了组合控件的重复利用。
	- 继承自原始控件：比方说正常的textview,我们希望在其下方加一条横线，就创建一个类继承textview,在ondraw方法里面进行相应绘画。
	- 继承自view或者v iewgroup，相对复杂许多，可能需要根据特殊情境，比方说下拉刷新和上拉加载。
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
- ## View事件分发传递
	- 伪代码
		- ```java
		   public boolean dispatchTouchEvent(MotionEvent ev) {
		          boolean consume = false;//事件是否被消费
		          if (onInterceptTouchEvent(ev)){//调用onInterceptTouchEvent判断是否拦截事件
		              consume = onTouchEvent(ev);//如果拦截则调用自身的onTouchEvent方法
		          }else{
		              consume = child.dispatchTouchEvent(ev);//不拦截调用子View的dispatchTouchEvent方法
		          }
		          return consume;//返回值表示事件是否被消费，true事件终止，false调用父View的onTouchEvent方法
		      }
		  ```
	-
- ## View动画
	- 动画分为帧动画，view动画，属性动画
	- ### 帧动画
		- 就和电视一样通过加载Drawable资源将逐个逐个图片进行拼接
		- 可以设置动画是否重复播放以及每个图片的持续时间
		- #### 帧动画优化
			- `SurfaceView`可以精细地控制帧动画每一帧的绘制，在每一帧绘制前才解析当前帧，且解析后续帧时复用前帧内存空间。遂整个过程在内存只申请了一帧图片大小的空间。
	- ### view动画（补间动画）
		- 由移动，缩放，透明度，旋转组成四种类型的补间动画；并且View动画还提供了动画集合类（AnimationSet），通过动画集合类（AnimationSet）可以将多个补间动画以组合的形式显示出来。
		- animationSet还可以实现监听，监听动画开始，动画取消，动画结束，动画重复执行
		- 但是补间动画只是实现了视觉效果上做了移动、缩放、旋转和淡入淡出的效果，其实并没有真正改变 View 的属性。
	- ### 属性动画
		- 相当于是view动画的增强版，除了view动画的四种基本方式，还有有其他复杂的动画可以供直接调用
		- 属性动画不光实现了视觉上的移动，其点击事件位置也跟随发生变化。、
- ## View更新
	- View的更新方式主要有以下3种：
	- 1.不使用多线程和双缓冲
		- 这种情况最简单，在View发生改变时对UI进行重绘。你只需要Activity中显式调用View对象中的invalidate()方法即可，系统会自动调用View的onDraw()方法。
	- 2.使用多线程和不使用双缓冲
		- 通过Handler对象，在子线程发送消息，在主线程处理消息更新UI。
	- 3.使用多线程和双缓冲
		- Android的SurfaceView是View的子类，它同时也实现了双缓冲。你可以定义一个它的子类并实现Surfaceholder.Callback接口。由于SurfaceHolder.Callback接口，新线程就不要android.os.Handler帮忙了。SurfaceHolder中lockCanvas()方法可以锁定画布，绘制完新的图像后调用unlockCanvasand Post解锁。
- ## 面试
	- ### 子view宽高可以超过父view？
		- 能，android:clipChildren = "false" 这个属性要设置在父 view 上。代表其中的子View 可以超出屏幕。
	- ### 自定义View需要注意哪些东西？
		- 1.让view支持wrap_content属性，在onMeasure方法中针对AT_MOST模式做专门处理，否则wrap_content会和match_parent效果一样（继承ViewGroup也同样要在onMeasure中做这个判断处理）
		- if(widthMeasureSpec == MeasureSpec.AT_MOST && heightMeasureSpec == MeasureSpec.AT_MOST){
		- setMeasuredDimension(200,200);
		- 2.view中有线程或者动画 要及时停止，避免内存泄漏
	- ### onTouch,onTouchEvent,onClick顺序优先级
		- onTouch方法返回true，则onTouchEvent方法不会被调用（onClick事件是在onTouchEvent中调用）所以三者优先级是onTouch->onTouchEvent->onClick
	- ### 自定义View如何配置xml选项
		- 设置自定义属性：
		- 创建styleable文件中创建自定义属性对应的变量类型，比方说text对应string
		- 再在自定义view的构造器利用TypedArray的方法将布局中的自定义属性对应的styleable文件解析出来并加以利用
	- ### Activity,Windows,View之间的关系？
		- Activity 主要管理生命周期 windows 首要负责窗口绘制 view主要是绘制内容
		- setContentView()实际上调用的是getWindow().setContentView()
	- ### PopupWindow和Dialog有什么区别？
		- 两者最根本的区别在于有没有新建一个window，PopupWindow没有新建，而是将view加到DecorView；Dialog是新建了一个window，相当于走了一遍Activity中创建window的流程
	- ### NestSrollView嵌套RecyclerView？
		- 要注意RecyclerView需要是固定高度，如果是wrap_content，则默认RecyclerView所有item都显示了，没了复用效果也没了滑动，只有NestedScrollView的滑动。
	- ### View和ViewGroup的区别？
		- 他俩的区别就在于重写其中的onMeasure、onlayout、ondraw三个方法。
		  viewgroup大部分情况不需要绘制，而view不需要layout。