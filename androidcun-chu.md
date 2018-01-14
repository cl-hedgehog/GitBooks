* ### Android中数据存储方式

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

2. 文件存储



