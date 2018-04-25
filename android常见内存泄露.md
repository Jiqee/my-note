### 一、内存泄露的产生原因：
    在Android程序开发中，当一个对象已经不需要再使用了，本该被回收时，而另外一个正在使用的对象持有它的引用从而导致它不能被回收，这就导致本该被回收的对象不能被回收而停留在堆内存中，内存泄漏就产生了。

### 二、java 引用的分类：
    分为4种：强引用、软引用、弱引用和虚引用。java jdk提供了三个专门的类，分别对应关系是：SoftReference---软引用；WeakReference---弱引用；PhantomReference---虚引用。
    1、强引用-----Strong reference：
    形式如同：A a = new A()这种；
    强引用本身存储在栈内存中，其存储指向堆内存中对象的地址。一般情况只有在没有强引用的情况下，垃圾回收站才会考虑对其内存进行回收。常见办法：如在不用该对象时将a置空，a = null。
    2、软引用-----Soft refernce:
    形式如同：
    A a = new A();//先new 一个强引用的对象
    SoftReference<A> srA = new SoftReference<A>(a);//通过对象的强引用，创建一个SoftReference对象，并将栈内存中的srA指向该对象
    a = null;//去掉强引用
    SoftReference变相的延长了其指示对象占据堆内存的时间，直到虚拟机内存不足时垃圾回收器才回收此堆内存空间。
    3、弱引用-----WeakReference:
    形式跟上面差不多：
    A a = new A();
    WeakReference<A> wrA = new WeakReference<A>(a);
    特点：WeakReference不改变原有强引用对象的垃圾回收时机，一旦其指示对象没有任何强引用对象时，该对象即进入正常的垃圾回收流程，有效降低内存泄露的风险。
    常见情况：在activity里面传递Hander的场景，在消息未传递完成，而activity退出时，消息队列持有hander实例的引用，而hander持有activity的引用，导致activity的内存资源无法回收，引发内存泄露。
    4、虚引用-----PhantomReference:
    与SoftReference或WeakReference相比，PhantomReference主要差别体现在如下几点：
    (1).PhantomReference只有一个构造函数PhantomReference(T referent, ReferenceQueue<? super T> q)，因此，PhantomReference使用必须结合ReferenceQueue；
    (2).不管有无强引用指向PhantomReference的指示对象，PhantomReference的get()方法返回结果都是null。
    与WeakReference相同，PhantomReference并不会改变其指示对象的垃圾回收时机。且可以总结出：ReferenceQueue的作用主要是用于监听SoftReference/WeakReference/PhantomReference的指示对象是否已经被垃圾回收。
    
### 三、常见内存泄露情况：
    1、不规范的单例
    单例是我们在平时做开发的时候经常用的设计模式，但其静态特性会导致其单例对象的生命周期和应用的生命周期一样长，用到Context这个属性时尤其需要注意。context有两种类型：activity的context(这个context跟当前acitvity是关联的)和application的context(该context属于全局的，只跟应用本身关联)。如果单例对象传入的是activity的context,在该activity退出时，而当前单例还在使用的情况，当前单例对象里面的context持有已退出的activity引用，会导致activity内存无法回收。如果需要传入context，最好传入application里面的context对象。
    2、非静态的内部类创建静态实例
    非静态内部类默认持有外部类的引用，而用该内部类创建的静态实例却和应用的生命周期一样长，会导致外部类的内存资源一直无法正常回收。正确的做法：将该内部类改为静态内部类，或者将该内部类抽取封装成一单例。
    3、Handler的使用
    平时我们经常用Handler来处理耗时操作传值，一般情况下都是直接用的非静态Handler对象，这样做会导致当前的activity在消息队列在loop线程里面轮询的情况下无法正常回收内存。建议采用封装一个Handler静态内部类，采用弱引用的方式去实例化当前activity对象。弱引用可以正常通过系统gcc来回收内存，不会内存泄露。
不过loop线程里面可能还有未执行的待发消息，建议在onDestroy或者onStop里面移除这些消息。
    4、线程造成的内存泄露
    无论是AsynTask还是Thread，在用作内部类来进行操作时，遇到线程任务没结束，都会持有外部类引用，造成外部类无法正常回收内存。建议
    使用静态的内部类。基于节约系统资源的目的，主动取消不需要继续执行的任务线程。
    5、资源未关闭
    像一些如BraodcastReceiver、file、contentObserver、cursor、Stream、Bitmap等，这些资源一般不会系统自动回收，需要我们在activity结束时自行关闭。
