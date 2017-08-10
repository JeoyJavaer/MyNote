
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

