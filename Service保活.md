------
title:Service保活的常见三种方式
------

### 一、三种方式介绍
	1、提高Service的优先级
	2、提高Service进程的优先级
	3、onDestroy方法里面重启Service

### 二、进程的相关概念
#### 1、优先级概述：
	Android系统在内存不足的情况下不可避免会kill掉一些优先级低的进程，这个时候就体现出优先级的重要性。

#### 2、优先级的5个常见等级：
	1.1、前台进程
	(1) 进程持有一个正在与用户交互的Activity;
	(2) 进程持有一个Service,这个Service处于这几种状态：
	 	Service与用户正在就交互的Activity绑定；
	 	Service是在前台运行的，即它调用了startForeground;
	 	Service正在执行它的生命周期回调函数(onCreate(),onStart(),or onDestroy())。
	(3) 进程持有一个BroadcastReceiver,这个BroadcastReceiver正在执行它的onReceive()方法。

	1.2、可见进程
	(1) 进程持有一个Activity,这个Activity处于onPause()状态，而还没调用onStop();
	(2) 进程持有一个Service,这个Service和一个可见(或者前台的)Activity绑定。

	1.3、服务进程
	类似于startService()调起的Service，同时与上面两种进程不相关，常见的如播放音乐。

	1.4、后台进程
	不属于上面的三种情况，进程持有一个用户不可见的activity(activity的onStop()被调用，但是onDestroy()没有调用的状态)

	1.5、空进程
	不含任何活跃应用组件的进程，就是空进程。

#### 3、新开线程的区别
	(1) 通过activity新开一个线程做耗时操作，然后当activity退出到桌面或者其他情况时会变成一个后台进程；
	(2) 通过service新开一个线程做耗时操作，相同情况下会变成一个服务进程
	注：服务线程肯定比后台进程优先级高，例如上传图片的操作，建议用服务进程来做。

### 三、常见保活手段
	1、在AndroidManifest.xml清单文件里面目标组件添加intent-filter属性，配置为
		android:priority="1000" -----1000表示最高优先级

	2、在onStartCommand方法里面调用startForeground()把Service提升为前台进程级别，然后在onDestroy里面调用stopForeground()。

	3、在onStartCommand()返回START_STICKY,这种只能保证当前进程没被干掉的情况下，等有内存时重启service。

	4、在onDestroy()方法里发广播重启service。

	5、监听系统广播判断Service状态，像重启之类的广播，接收到后重启service。

	6、Application加上Persistent属性

