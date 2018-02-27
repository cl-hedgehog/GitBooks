需求不同，问题不同

* java注解和反射机制
* glide会根据activity的生命周期来加载的源码实现。
* 插件化的实现思路。
* android系统为何要设计handler这样的通信机制。
* tcp/http原理，java源码、算法
* android各种组件的源码以及各种开源框架的源码
* ~~Java的垃圾回收机制~~
* HTTP的网络连接
* Okhttp实现
* 图片三级缓存
* 设计模式、工作中遇到的困难

一些问题：

1-数据层有统一的管理么，数据缓存是怎么做的，http请求等有提供统一管理么？

2-有用什么模式么，逻辑什么的都在Activity层？怎么分离的

3-如果用了一些解耦的策略，怎么管理生命周期的？

4-有什么提高编译速度的方法？

5-对应用里的线程有做统一管理么？

6-jni的算法提供都是主线程的？是不是想问服务类的啊

7-边沿检测用的啥？深度学习相关的有了解么？

8-上线后的app性能分析检测有做么

DiDi:一个多小时，比较全面

1. ActivityA跳转ActivityB然后B按back返回A，各自的生命周期顺序，A与B均不透明。
2. **Synchronize关键字后面跟类或者对象有什么不同。**
3. **单例的DCL方式下，那个单例的私有变量要不要加volatile关键字，这个关键字有什么用**
4. JVM的引用树，什么变量能作为GCRoot？GC垃圾回收的几种方法
5. ThreadLocal是什么？Looper中的消息死循环为什么没有ANR？
6. **Android中main方法入口在哪里**
7. **jdk1.5？SparseArray和ArrayMap各自的数据结构，前者的查找是怎么实现的，与HashMap的区别**
8. **Runnable与Callable、Future、FutureTask的区别，AsyncTask用到哪个？AsyncTask是顺序执行么，for循环中执行200次new AsyncTask并execute，会有异常吗**
9. **IntentService生命周期是怎样的，使用场合等**
10. **RecyclerView和ListView有什么区别？局部刷新？前者使用时多重type场景下怎么避免滑动卡顿。懒加载怎么实现，怎么优化滑动体验。**
11. **SQLite的数据库升级用过么**
12. **开放问题：如果提高启动速度，设计一个延迟加载框架或者sdk的方法和注意的问题。**
13. **Scroller有什么方法，怎么使用的。**
14. **分享下项目中遇到的问题**
15. **webwiew了解？怎么实现和javascript的通信？相互双方的通信。@JavascriptInterface在？版本有bug，除了这个还有其他调用android方法的方案吗？**
16. **ReactiveNative了解多少**
17. **JNI和NDK熟悉么？Java和C方法之前的相互调用怎么做？**



