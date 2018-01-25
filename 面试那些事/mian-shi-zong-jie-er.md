### 二、机会留给准备好的人

一定不要灰心啊，要时刻准备着，因为你不知道什么时候就遇到机会了，不抓住只有更悔恨，机会留给准备好的人说的没错，坚持。

另一个大厂，比较有深度：

1. Canvas的底层机制，绘制框架，硬件加速是什么原理，canvas lock的缓冲区是怎么回事

2. surfaceview， suface，surfacetexure等相关的，以及底层原理

3. android文件存储，各版本存储位置的权限控制的演进，外部存储，内部存储

4. 上层业务activity和fragment的遇到什么坑？？页面展示上的一些坑和优化经验

yz: 只写新的

* 进程间通信方式？Binder的构成有几部分？
* HttpClient和HttpConnection的区别
* View的事件传递机制
* MVC，MVP，MVVM分别是什么？
* Android中常用的设计模式，说三个比较高级的？
* 内存优化，OOM的原因和排查方法
* 想改变listview的高度，怎么做
* 异常是怎么捕获的，CrashHandler是怎么写的？
* 除了日常开发，其他有做过什么工作？比如持续化集成，自动化测试等等

zfb：工程技术部研发协作：钱包/工具类/集成插件/自动化集成和打包

比如：更改打包配置，更换icon，debug/release等

* glide缓存策略？同一个图片跟size有关么
* android中的动画有哪些
* View事件传递机制
* 界面卡顿怎么排查和优化？
* Fragment的replace和end？？的区别？
* MVP，MVVM，MVC解释和实践
* 项目之外的，对技术的见解，拓展知识

二面：

* 微信跳一跳外挂怎么实现，检测怎么做的？
* 一张纯色背景下怎么有效检测各个矩形？
* 对接的so算法了解么，有接触过相关的库么？

openCV，GPUImage

* 专业课哪些比较好，有利用到自己的工作中么？

矩阵、计算几何相关的课程。利用矩阵变换、bezier曲线，等距曲线等知识解决轨迹贴纸中的问题。计算机视觉中的相机坐标系与opengl中的各种坐标系变换。

* 三个算法题并写出测试用例：打印n-m之间所有的素数；计算n-m之间1出现的次数；指定数字序列的排序；
* android api层的源码熟悉哪些？解释一下

消息机制-可以提到死循环效率问题。

属性动画中有个虚拟机执行引擎的动态分派原则（子类方法重写）。

LinearLayout和RelativeLayout的测量过程，前者对于weight的处理。

AsyncTask的两个线程池。

* ACTION\_CANCEL什么时候触发，触摸button然后滑动到外部抬起会触发点击事件吗，在滑动回去抬起会么

都不会。当控件收到前驱事件（什么叫前驱事件？一个从DOWN一直到UP的所有事件组合称为完整的手势，中间的任意一次事件对于下一个事件而言就是它的前驱事件）之后，后面的事件如果被父控件拦截，那么当前控件就会收到一个CANCEL事件，并且把这个事件会传递给它的子事件。（注意：这里如果在控件的onInterceptTouchEvent中拦截掉CANCEL事件是无效的，它仍然会把这个事件传给它的子控件）之后这个手势所有的事件将全部拦截，也就是说这个事件对于当前控件和它的子控件而言已经结束了。

比如，当你的手指（或者其它）移动屏幕的时候会触发这个事件，比如当你的手指在屏幕上拖动一个listView或者一个ScrollView而不是去按上面的按钮时会触发这个事件。在设计设置页面的滑动开关时，如果不监听ACTION\_CANCEL，在滑动到中间时，如果你手指上下移动，就是移动到开关控件之外，则此时会触发ACTION\_CANCEL，而不是ACTION\_UP，造成开关的按钮停顿在中间位置。意思就是，当用户保持按下操作，并从你的控件转移到外层控件时，会触发ACTION\_CANCEL，建议进行处理～当前的手势被中断，不会再接收到关于它的记录。推荐将这个事件作为 ACTION\_UP 来看待，但是要区别于普通的 ACTION\_UP。

[android MotionEvent.ACTION\_CANCEL情景分析](http://blog.csdn.net/y444400/article/details/53435696)

题目中这种场景就是：是父视图的onInterceptTouchEvent先返回false，子控件消费事件（dispatchTouchEvent返回true）,然后在某种情况下onInterceptTouchEvent返回true，子控件就会收到ACTION\_CANCEL的Event。DOWN传递给了子控件，但是滑动的时候，父控件的onInterceptTouchEvent返回true，导致子控件的前驱事件被拦截，收不到了，就会收到一个ACTION\_CANCEL的Event。在ViewGroup的一段代码里可以看出。

```java
            // Handle an initial down.
            if (actionMasked == MotionEvent.ACTION_DOWN) {
                // Throw away all previous state when starting a new touch gesture.
                // The framework may have dropped the up or cancel event for the previous gesture
                // due to an app switch, ANR, or some other state change.
                cancelAndClearTouchTargets(ev);
                resetTouchState();
            }

            // Check for interception.
            final boolean intercepted;
            if (actionMasked == MotionEvent.ACTION_DOWN
                    || mFirstTouchTarget != null) {
                final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
                if (!disallowIntercept) {
                    intercepted = onInterceptTouchEvent(ev);
                    ev.setAction(action); // restore action in case it was changed
                } else {
                    intercepted = false;
                }
            } else {
                // There are no touch targets and this action is not an initial down
                // so this view group continues to intercept touches.
                intercepted = true;
            }

            // If intercepted, start normal event dispatch. Also if there is already
            // a view that is handling the gesture, do normal event dispatch.
            if (intercepted || mFirstTouchTarget != null) {
                ev.setTargetAccessibilityFocus(false);
            }

            // Check for cancelation.
            final boolean canceled = resetCancelNextUpFlag(this)
                    || actionMasked == MotionEvent.ACTION_CANCEL;
```

* 怎么处理嵌套View的滑动冲突问题

* 热修复相关的原理，框架熟悉么
* gradle打包流程熟悉么
* 任意提问环节：其实可以问之前面试中遇到的问题：比如，多模块开发的时候不同的负责人可能会引入重复资源，相同的字符串，相同的icon等但是文件名并不一样，怎样去重？



