title: Android HanderTheader及其退出方法
id: 42
categories:
  - 奇怪的
date: 2014-10-29 15:32:10
tags:
---

如果需要在主线程之外开辟一个线程运行一些代码的，可以用HanderTheader。表达的不好，直接看代码

创建方法：
<pre lang="java" line="1" escaped="true">
HandlerThread handlerThread = new HandlerThread("mythread");
handlerThread.start();
Looper looper = handlerThread.getLooper();
mHandler = new Handler(looper);
</pre>
接着按着正常用Hander的方法给他发消息让另外一个线程运行代码了
new Handler的时候，可以new一个Handler类的自定义子类，进行定制
注意如果我们start了一个HandlerThread，那么这个线程会启动一个looper消息循环，当界面退出等原因不再调用它的时候，这个HandlerThread线程并没有终止，还是在那里做looper死循环。如果因为界面重新载入等方式重新new了一个又一个Looper，会加剧对资源的消耗。需要对其进行销毁。

销毁方法：
<pre lang="java" line="1" escaped="true">
mHandler.getLooper().quit();
</pre>