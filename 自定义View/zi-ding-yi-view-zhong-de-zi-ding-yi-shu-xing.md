### 

### 自定义View的属性

1. 首先在工程目录的res/values/attrs.xml中创建属于自己的自定义控件的属性集合。

```java
<?xml version="1.0" encoding="UTF-8"?>
<resources>
    <declare-styleable name="RoundProgressBar">
        <attr name="roundColor" format="color|reference"/>
        <attr name="roundProgressColor" format="color|reference"/>
        <attr name="roundWidth" format="dimension|reference"/>
        <attr name="roundProgressWidth" format="dimension|reference"/>
        <attr name="circleSize" format="dimension|reference"/>
        <attr name="textSize" format="dimension|reference"/>
        <attr name="textColor" format="color|reference"/>
        <attr name="max" format="integer"/>
        <attr name="textIsDisplayable" format="boolean"/>
        <attr name="style">
            <enum name="STROKE" value="0"/>
            <enum name="FILL" value="1"/>
        </attr>
    </declare-styleable>
</resources>
```

比如自定义控件的类名叫做_RoundProgressBar_，它在XML定义的时候可以指定的属性有以上定义的那么多，各种属性可以设置的值是在format中指定的，而且支持枚举形式，那么自定义的类中怎么获取到这些属性并在代码中使用呢？

1. 自定义View类中获取属性

下面列出构造函数中使用_TypedArray_对属性值的获取，初始化时加载了上一步中的自定义属性集合，用完记得调用_recycle\(\)_。

```java
public RoundProgressBar(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        TypedArray mTypedArray = context.obtainStyledAttributes(attrs, R.styleable.RoundProgressBar);
        //获取自定义属性和默认值
        roundColor = mTypedArray.getColor(R.styleable.RoundProgressBar_roundColor, Color.WHITE);
        roundProgressColor = mTypedArray.getColor(R.styleable.RoundProgressBar_roundProgressColor, Color.RED);
        textColor = mTypedArray.getColor(R.styleable.RoundProgressBar_textColor, Color.WHITE);
        circleSize = mTypedArray.getDimensionPixelSize(R.styleable.RoundProgressBar_circleSize, 40);
        textSize = mTypedArray.getDimensionPixelSize(R.styleable.RoundProgressBar_textSize, 18);
        roundWidth = mTypedArray.getDimensionPixelSize(R.styleable.RoundProgressBar_roundWidth, 5);
        roundProgressWidth = mTypedArray.getDimensionPixelSize(R.styleable.RoundProgressBar_roundProgressWidth, 8);
        max = mTypedArray.getInteger(R.styleable.RoundProgressBar_max, 30);
        textIsDisplayable = mTypedArray.getBoolean(R.styleable.RoundProgressBar_textIsDisplayable, true);
        style = mTypedArray.getInt(R.styleable.RoundProgressBar_style, 0);
        mTypedArray.recycle();
        // 计算逻辑
        center = circleSize / 2; //获取圆心的x坐标
        radius = (int) (center - (roundWidth + roundProgressWidth) / 2); //圆环的半径
        //用于定义的圆弧的形状和大小的界限
        oval = new RectF(center - radius, center - radius, center + radius, center + radius);
        paint = new Paint();
    }
```

这些字段经过上面的获取，后续的使用中就有了XML中设置的值或者默认值。

1. 关于属性中跟size相关的几个函数

介绍下几个获取size的函数，注意选取正确的函数获取px，dp等单位对应的值。

```java

    private void test() {
        float a1=getResources().getDimension(R.dimen.activity_vertical_margin1);
        int a2=getResources().getDimensionPixelOffset(R.dimen.activity_vertical_margin1);
        int a3=getResources().getDimensionPixelSize(R.dimen.activity_vertical_margin1);

        float b1=getResources().getDimension(R.dimen.activity_vertical_margin2);
        int b2=getResources().getDimensionPixelOffset(R.dimen.activity_vertical_margin2);
        int b3=getResources().getDimensionPixelSize(R.dimen.activity_vertical_margin3);

        float c1=getResources().getDimension(R.dimen.activity_vertical_margin3);
        int c2=getResources().getDimensionPixelOffset(R.dimen.activity_vertical_margin3);
        int c3=getResources().getDimensionPixelSize(R.dimen.activity_vertical_margin3);

        Log.i("test", "getDimension= "+a1+", getDimensionPixelOffset="+a2+",getDimensionPixelSize="+a3);
        Log.i("test", "getDimension= "+b1+", getDimensionPixelOffset="+b2+",getDimensionPixelSize="+b3);
        Log.i("test", "getDimension= "+c1+", getDimensionPixelOffset="+c2+",getDimensionPixelSize="+c3);
    }
```

测试手机density=3。其中

```java
    <dimen name="activity_vertical_margin1">16dp</dimen>
    <dimen name="activity_vertical_margin2">16px</dimen>
    <dimen name="activity_vertical_margin3">16sp</dimen>
```

打印结果是：

> 01-13 23:53:44.476 8317-8317/? I/test: getDimension= 48.0, getDimensionPixelOffset=48,getDimensionPixelSize=48
>
> 01-13 23:53:44.476 8317-8317/? I/test: getDimension= 16.0, getDimensionPixelOffset=16,getDimensionPixelSize=48
>
> 01-13 23:53:44.477 8317-8317/? I/test: getDimension= 48.0, getDimensionPixelOffset=48,getDimensionPixelSize=48

根据api解释是

getDimension返回float

getDimensionPixelOffset返回int，是float强转后的int，会丢失小数。

getDimensionPixelSize返回int，不过是经过四舍五入的int。

返回的数字单位都是px，看源码仍然没有明白的是对于px的单位，getDimensionPixelOffset比较精确，没有乘以density，但是其他两个函数用px为单位的时候，也乘以了density。

源码的分析可以参考这一篇：[Android Dimension转换算法原理分析](http://blog.csdn.net/xilove102/article/details/52702908)

从分析也可以得知，sp之所以可能随着系统字体设置大小而改变，也是计算的时候乘以了个系数。

### 常用的获取控件大小的方法



