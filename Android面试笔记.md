
1.listview优化

	a:复用convertview b: viewholder c:分页加载 d:分批加载

2.Android  oom产生的原因 及处理

	原因： a: 应用中加载大对象，如bitmap b:长期保存某些大资源的引用。导致内存泄漏（静态变量，context不合理使用，非静态内部类，drawable，数据库，file流）
	处理：加载大bitmap 进行压缩，设置BitmapFactory.Optiions的inJustDecodeBounds（边界压缩）属性为true，Options.inSampleSize 可以设置压缩比。或者开源图片加载框架

3.AsyncTask 原理

	a:onPreExecute（） b:doInBackground（Param。。）c:onProgressUpdate（Progress。。。）d:onPostExecute（Result。。。）
	规则：AsyncTask必须在UI线程中创建，execute方法必须在UI线程调用

4.四大组件

	通过startService()方法启动的服务与调用者没有关系,即使调用者关闭了,服务仍然运行想停止服务要调用Context.stopService(),此时系统会调用onDestory(),使用此方法启动时,服务首次启动系统先调用服务的onCreate()-->onStartCommond(),如果服务已经启动再次调用只会触发onStart()Commod()方法,销毁时调用onDestroy();方法。使用bindService()启动的服务与调用者绑定,只要调用者关闭服务就终止,使用此方法启动时,服务首次启动系统先调用服务的onCreate()-->onBind(),如果服务已经启动再次调用不会再触发这2个方法,调用者退出时系统会调用服务的onUnbind()-->onDestory(),想主动解除绑定可使用Contex.unbindService(),系统依次调用onUnbind()-->onDestory();

	 动态注册广播接收器还有一个特点，就是当用来注册的Activity关掉后，广播也就失效了

5.事件传递

	在View和ViewGroup中都存在dispatchTouchEvent和onTouchEvent方法，但是在ViewGroup中还有一个onInterceptTouchEvent方法。

	1，Android中事件传递按照从上到下进行层级传递，事件处理从Activity开始到ViewGroup再到View。2，事件传递方法包括dispatchTouchEvent、onTouchEvent、onInterceptTouchEvent，其中前两个是View和ViewGroup都有的，最后一个是只有ViewGroup才有的方法。这三个方法的作用分别是负责事件分发、事件处理、事件拦截。***3，onTouch事件要先于onClick事件执行，onTouch在事件分发方法dispatchTouchEvent中调用，而onClick在事件处理方法onTouchEvent中被调用，onTouchEvent要后于dispatchTouchEvent方法的调用**。*

**6.aidl**

	Android Interface Definition Language 作用：当服务调用者和服务不在同一个应用应用，此时服务调用者想通过远程绑定服务调用服务里面的方法就必须通过aidl技术

7.设计模式
	总体分为三大类：
		创建型模式：共五种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
		结构型模式，共七种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
		行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式

8.java中如何实现序列化，有什么意义？

	序列化是为了解决对象流读写操作时可能引发的问题（不进行序列化，可能引起数据混乱）。序列化除了能够实现对象的持久化外，还能够用于对象的深度克隆。

9.view 的绘制流程
	
	[http://blog.csdn.net/yanbober/article/details/46128379/](http://blog.csdn.net/yanbober/article/details/46128379/ "Android 应用层view的绘制流程")

	整个绘制流程是在viewroot中的performTraversals（）执行
			private void performTraversals() {
				......
				//最外层的根视图的widthMeasureSpec和heightMeasureSpec由来
				//lp.width和lp.height在创建ViewGroup实例时等于MATCH_PARENT
				int childWidthMeasureSpec = getRootMeasureSpec(mWidth, lp.width);
				int childHeightMeasureSpec = getRootMeasureSpec(mHeight, lp.height);
				......
				mView.measure(childWidthMeasureSpec, childHeightMeasureSpec);//测量
				......
				mView.layout(0, 0, mView.getMeasuredWidth(), mView.getMeasuredHeight());//布局
				......
				mView.draw(canvas);//绘制
				......
			}

		这里出现一个问题，getWidth/Height() and getMeasuredWidth/Height()有什么区别？

		getWidth():View在設定好佈局後整個View的宽度。
		getMeasuredWidth():對View上的內容進行測量後得到的View內容占据的宽度 ，包括影藏的布局
		
		Draw过程   public void draw(Canvas canvas) {
						        ......
						        /*
						         * Draw traversal performs several drawing steps which must be executed
						         * in the appropriate order:  视图的绘制流程
						         *
						         *      1. Draw the background   1，背景
						         *      2. If necessary, save the canvas' layers to prepare for fading
						         *      3. Draw view's content  
						         *      4. Draw children  
						         *      5. If necessary, draw the fading edges and restore layers  5，阴影
						         *      6. Draw decorations (scrollbars for instance)   6，装饰
						         */
						
						        // Step 1, draw the background, if needed
						        ......
						        if (!dirtyOpaque) {
						            drawBackground(canvas);
						        }
						
						        // skip step 2 & 5 if possible (common case)
						        ......
						
						        // Step 2, save the canvas' layers
						        ......
						            if (drawTop) {
						                canvas.saveLayer(left, top, right, top + length, null, flags);
						            }
						        ......
						
						        // Step 3, draw the content
						        if (!dirtyOpaque) onDraw(canvas);
						
						        // Step 4, draw the children
						        dispatchDraw(canvas);
						
						        // Step 5, draw the fade effect and restore layers
						        ......
						        if (drawTop) {
						            matrix.setScale(1, fadeHeight * topFadeStrength);
						            matrix.postTranslate(left, top);
						            fade.setLocalMatrix(matrix);
						            p.setShader(fade);
						            canvas.drawRect(left, top, right, top + length, p);
						        }
						        ......
						
						        // Step 6, draw decorations (scrollbars)
						        onDrawScrollBars(canvas);
						        ......
						    }


10. 安卓中的动画

	3.0以前只有2中  3.0之后3中 帧动画，补间动画（视图动画），属性动画

11. aidl  安卓接口定义语言
	
	[http://blog.csdn.net/lmj623565791/article/details/38377229](http://blog.csdn.net/lmj623565791/article/details/38377229)
	
12.Android binder机制原理 
	[http://blog.csdn.net/boyupeng/article/details/47011383](http://blog.csdn.net/boyupeng/article/details/47011383)

		Binder是Android系统进程间通信（IPC）方式之一。Linux已经拥有的进程间通信IPC手段包括(Internet Process Connection)： 管道（Pipe）、信号（Signal）和跟踪（Trace）、插口（Socket）、报文队列（Message）、共享内存（Share Memory）和信号量（Semaphore）。

13.ANR 及如何避免	
	
	三种事件类型： a:按键或者触摸事件  5s   b: 广播 10s  c:服务20s
	关键：避免在ui线程做复杂操作或者计算
	定位：。。/data/and/traces.txt

14. android 系统架构 

	linux kernel   lib dalvik vm   application framework  app

15.framework工作原理 ，activity是如何生成一个view的，机制是什么
	
	Framework是android 系统对 linux kernel，lib库等封装，提供WMS，AMS，bind机制，handler-message机制等方式，供app使用。简单来说framework就是提供app生存的环境。
		1）Activity在attch方法的时候，会创建一个phonewindow（window的子类）	
		2）onCreate中的setContentView方法，会创建DecorView
		3）DecorView 的addview方法，会把layout中的布局加载进来。