* ### Android中数据存储方式

官网上[存储选项](https://developer.android.com/guide/topics/data/data-storage.html)章节翻译的五种存储形式：

1.共享首选项

2.内部存储

3.外部存储

4.SQLite数据库

5.网络连接

常用的五种存储方式：[Android数据存储五种方式总结](http://blog.csdn.net/sunny_lv/article/details/51383546)

1 使用SharedPreferences存储数据

2 文件存储数据

3 SQLite数据库存储数据

4 使用ContentProvider存储数据

5 网络存储数据

下面就列出关键的知识点，具体使用和解释可以参见上面转载的博客。

1. SharedPreferences

![](/assets/Android-sp.png)

其中，Editor的commit和apply的区别的参考：[SharedPreferences中的commit和apply方法](https://www.jianshu.com/p/c8d10357c939)

拓展：之前被问到SharedPreferences中能否存Bitmap，显然直接是不可以的，但是如果Bitmap比较小，可以将它转换为字节流，然后用Base64转为String对象存储，因为Sp支持的类型有限，只能转为String去处理。

[SharedPreferences存储图片对象与获取](http://blog.csdn.net/simon_crystin/article/details/52454593)

将Bitmap对象变转化成64位字节流，存储到本地

```java
SharedPreferences sharedPreferences=getSharedPreferences("headPic", Activity.MODE_PRIVATE);
SharedPreferences.Editor editor=sharedPreferences.edit();
ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();
bitmap.compress(Bitmap.CompressFormat.PNG, 50, byteArrayOutputStream);
String headPicBase64=new String(Base64.encodeToString(byteArrayOutputStream.toByteArray(),Base64.DEFAULT));
editor.putString("headPic",headPicBase64);
editor.commit();
```

从本地获取64位字节流对应的字符串，并解析成图片对象

```java
SharedPreferences sharedPreferences=getSharedPreferences("headPic",Activity.MODE_PRIVATE);
String headPic=sharedPreferences.getString("headPic","");
Bitmap bitmap=null;
if (headPic!="") {
     byte[] bytes = Base64.decode(headPic.getBytes(),1);
     //  byte[] bytes =headPic.getBytes();
     bitmap= BitmapFactory.decodeByteArray(bytes, 0, bytes.length);
}
```

然后接着有面试官会问Base64编码的问题，这个会在其他单元总结下各种常用的编解码和加密解密。简单说Base64算不上加密解密领域里的算法，但是是一种常用的编解码手段。

1. 文件存储

[彻底了解android中的内部存储与外部存储](http://www.cnblogs.com/jingmo0319/p/5586559.html)





几个跟api版本有关的备注：

1. 文件存储中的模式设置

**注**：自 API 级别 17 以来，常量[`MODE_WORLD_READABLE`](https://developer.android.com/reference/android/content/Context.html#MODE_WORLD_READABLE)和[`MODE_WORLD_WRITEABLE`](https://developer.android.com/reference/android/content/Context.html#MODE_WORLD_WRITEABLE)已被弃用。从 Android N 开始，使用这些常量将会导致引发[`SecurityException`](https://developer.android.com/reference/java/lang/SecurityException.html)。这意味着，面向 Android N 和更高版本的应用无法按名称共享私有文件，尝试共享“file://”URI 将会导致引发[`FileUriExposedException`](https://developer.android.com/reference/android/os/FileUriExposedException.html)。 如果您的应用需要与其他应用共享私有文件，则可以将[`FileProvider`](https://developer.android.com/reference/android/support/v4/content/FileProvider.html)与[`FLAG_GRANT_READ_URI_PERMISSION`](https://developer.android.com/reference/android/content/Intent.html#FLAG_GRANT_READ_URI_PERMISSION)配合使用。另请参阅[共享文件](https://developer.android.com/training/secure-file-sharing/index.html)。





