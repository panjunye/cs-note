# Activity的4种launch mode
### standard 
> 每次都会新创建一个Activity的实例，放到栈顶
### singleTop
> A-B-C-D，当D在顶部时，D收到Intent，并不会产生一个新的D的实例，而是会调用D的onNewIntent方法。但是即使B是singleTop，B收到一个Intent，也会重新创建一个B的实例

应用：防止快速点击，产生多个实例

3. singleTask
> 系统会创建一个新的Task，并且把Activity放在Task的栈底，如果Activity已经存在于其他的Task中，那么系统会调用该Activity实例的onNewIntent方法。

应用：设置在MainActivity，用户重新登录后，通过SingleTask和ClearTask启动标记可以返回到MainActivity

4. singleInstance
> 跟singleTask一样，不同的是Activity只会是一个单例，并且独占一个Task

应用： Home Screen


# Handler
> This Handler class should be static or leaks might occur: IncomingHandler

Handle的实现类应该为静态，因为同一个Thread的所有handler都共用一个Looper,而每一个Message都持有Handler的引用，handler又持有Activity的引用，因此如果Message还在多列中，那么Activity就不能为GC回收。
可看[Stackoverflow的解释](https://stackoverflow.com/questions/11407943/this-handler-class-should-be-static-or-leaks-might-occur-incominghandler)。

Handler机制：首先对于每一个线程都有一个Looper，Looper.prepare()方法向其内部的ThreadLocalc存放一个新的Looper，每个线程只能执行一次。每个Looper内部有一个MessageQueue，handler发消息时，发到Looper内部的MessageQueue。Looper.loop()方法会循环从队列中获取Message，取到Message后，在调用handler的handleMessage方法。

# 多线程的方式
1. new Thread()
2. AsyncTask
3. Handler
4. IntentService
5. ThreadPoolExecutor


# [ANR](https://developer.android.com/topic/performance/vitals/anr)

> Applicatoin Not Responding。ANR产生的原因是负责更新UI的线程，不能继续处理用户输入或者更新UI

### 产生原因
1. 5s内没有响应用户输入事件
2. 10s内广播接收器没有处理完毕

### 诊断方法
1. 在main线程内进形I/O操作
2. 在main线程进行耗时的运算
3. 同步binderd到另一个进程，该进程进行耗时操作

### Strict Mode
在Application中配置StrictMode

### traces.txt

> adb pull /data/anr/traces.txt

# GC

### 回收算法

- 引用计数法

- Tracing算法或标记-清除算法
    - 根搜索算法
    - Tracing算法
    - 标记清除算法
- 标记-整理算法
- Copying算法（复制算法）
- 分代算法 （Generation）
