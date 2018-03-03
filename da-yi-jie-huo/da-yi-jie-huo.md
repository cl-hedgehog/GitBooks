写在前面：

今天总结了几个内存相关的问题，感觉平时只注重使用并没有关注背后的原理和底层支持，使用上肯定不能涉及到这么多方面，但是如果用到的api都能从：怎么用？有哪些坑？背后的原理和原因？这些方面都花时间探究了的话，也不会在面试时这么被动，很多问题是有联系的，但是平时并没有总结，花在看视频八卦和思考人生的时间都比这些思考多，下一个阶段一定要更加严格，因为这些实践性的东西真的很容易遗忘，也很分散，如果真的想成为一名专业的优秀的高级工程师，在没有天赋的情况下，努力和产出远远不够，但要养成习惯的话就会离梦想越来越近。

* **弱引用与GC @20180120**

被自己绕进去的一个问题，看GC回收时说发生GC时，所有的弱引用都要被回收。GC其实分Minor GC和Major GC，两种情况下弱引用都会被回收的。

我打开一个定义了弱引用当前Activity的静态UIHandler的Activity页面，然后用Android的Memory Monitor去监测的时候点击了垃圾回收，发现debug状态下this指针中的这个UIHandler对象中的弱引用并没有被回收，顿时慌张了，怎么可能啊。过会才反应过来自己SB了，当前Activity还在运行着，肯定不能回收啊，也就是虽然是被弱引用引用，但是本身在GCRoots里是被强引用着的，只有当前只有弱引用的时候才会在GC的时候被回收，所以关闭Activity后如果有内存泄漏的风险，这种方法也会在系统GC的时候将对象回收掉，避免一直泄漏。
