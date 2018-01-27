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

ref1:[android MotionEvent.ACTION\_CANCEL情景分析](http://blog.csdn.net/y444400/article/details/53435696)

ref2:[关于MotionEvent.ACTION\_CANCEL带来的滑动问题解决](http://blog.csdn.net/kingofhacker/article/details/75111372)

step1. 父View收到ACTION\_DOWN，如果没有拦截事件，则ACTION\_DOWN前驱事件被子视图接收，父视图后续事件会发送到View。step2. 此时如果在父View中拦截ACTION\_UP或ACTION\_MOVE，在第一次父视图拦截消息的瞬间，父视图指定子视图不接受后续消息了，同时子视图会收到ACTION\_CANCEL事件。  
一般ACTION\_CANCEL和ACTION\_UP都作为View一段事件处理的结束。

几乎所有的自定义控件都要手动处理onTouchEvent事件,我们知道,onTouchEvent方法返回的布尔值决定了你是否处理\(消费当前事件\),但是这么笼统的说其实是不准确的.准确来说,是当手指按下,也就是onTouchEvent接收到ACTION\_DOWN事件的时候,如果返回true,那么就代表这次事件被我们处理,后来的ACTION\_MOVE和ACTION\_UP的返回结果是无所谓的. 同样的,如果在ACTION\_DOWN的时候我们返回了false,那么后来的事件也不会传到这里.

ACTION\_CANCLE事件如何被触发?

从表象来说明就是:当触摸事件从我们的控件开始,也就是ACTION\_DOWN被我们返回true处理,然后手指移动到当前控件的外面,这时候就会触发ACTION\_CANCLE事件,触发cancle事件就不会接收到up事件!!! 实际上当手指移动到了当前控件之外的时候,这个触摸事件被他的父控件拦截掉了,所以触发了cancle事件.之后的触摸事件就再也不会传递到当前控件的onTouchEvent里面.

ACTION\_CANCLE事件如何处理?关于处理,有两种办法:

1.如果是类似滑动开关,开关随着手指移动,当手指不小心移动到开关外面,我们可以对cancle进行判断，触发cancle的时候，将用户未做完的动作自动执行，也就是自动将开关滑动到左边或者右边。具体逻辑自行处理.如果你觉得这种不太好，那么请看第二种解决方案。

2.类似于自定义progressBar，触发cancle的时候没法进行自动处理。其实我们可以在onTouchEvent里面先调用requestDisallowInterceptTouchEvent\(true\);，这句代码的意思就是不允许父控件拦截我们的触摸事件，这样action\_canlce事件就永远不会被触发，问题也就不存在了。

ps：我看源码中在ViewGroup中，dispatchtouchEvent总，有一部分处理是关于点击的target的，mFirstTouchTarget的设置是如果有子View有处理touch down这个会被赋值，接下来的事件序列中，在ViewGroup不拦截的情况下回判断新的target和这个旧的是否一致，不一致的时候会执行到

```java
 // Dispatch to touch targets.
            if (mFirstTouchTarget == null) {
                // No touch targets so treat this as an ordinary view.
                handled = dispatchTransformedTouchEvent(ev, canceled, null,
                        TouchTarget.ALL_POINTER_IDS);
            } else {
                // Dispatch to touch targets, excluding the new touch target if we already
                // dispatched to it.  Cancel touch targets if necessary.
                TouchTarget predecessor = null;
                TouchTarget target = mFirstTouchTarget;
                while (target != null) {
                    final TouchTarget next = target.next;
                    if (alreadyDispatchedToNewTouchTarget && target == newTouchTarget) {
                        handled = true;
                    } else {
                        final boolean cancelChild = resetCancelNextUpFlag(target.child)
                                || intercepted;
                        if (dispatchTransformedTouchEvent(ev, cancelChild,
                                target.child, target.pointerIdBits)) {
                            handled = true;
                        }
                        if (cancelChild) {
                            if (predecessor == null) {
                                mFirstTouchTarget = next;
                            } else {
                                predecessor.next = next;
                            }
                            target.recycle();
                            target = next;
                            continue;
                        }
                    }
                    predecessor = target;
                    target = next;
                }
            }
```

> dispatchTransformedTouchEvent\(ev, cancelChild，target.child, target.pointerIdBits\)）

这个里面会

```java
if (cancel || oldAction == MotionEvent.ACTION_CANCEL) {
            event.setAction(MotionEvent.ACTION_CANCEL);
            if (child == null) {
                handled = super.dispatchTouchEvent(event);
            } else {
                handled = child.dispatchTouchEvent(event);
            }
            event.setAction(oldAction);
            return handled;
        }
```

给child发cancel事件，并返回其处理结果，感觉是这里起得作用。但是不确定，也没有验证，关于ScrollView中button的情况，ref1中的是对的，它重写的OnInterceptTouchEvent方法中需要处理滑动，所以交由自己处理了。

* 怎么处理嵌套View的滑动冲突问题

* 热修复相关的原理，框架熟悉么

* gradle打包流程熟悉么

* 任意提问环节：其实可以问之前面试中遇到的问题：比如，多模块开发的时候不同的负责人可能会引入重复资源，相同的字符串，相同的icon等但是文件名并不一样，怎样去重？



