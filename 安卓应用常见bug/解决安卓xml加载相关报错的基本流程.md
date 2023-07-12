Caused by: java.lang.IllegalStateException: The specified child already has a parent. You must call removeView() on the child's parent first.
这种错误一般是xml文件有问题导致
下面是具体分析过程

###1.查看log
![error上半部分](https://upload-images.jianshu.io/upload_images/24860325-975ecfcfb5a00122.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![error下半部分](https://upload-images.jianshu.io/upload_images/24860325-233815bdd9573996.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###看到蓝色的报错代码后，点击连接到报错代码
```
binding = ActivityLoginBinding.inflate(layoutInflater)
```

###这句代码本身没有什么问题，报错一般是因为加载的xml文件有误
回到log上半部分，提示xml中14行报错
```
app:navGraph="@navigation/login_navigation" />
```
###先看报错的activity布局

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/nav_host_fragment_activity_login"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/login_navigation" />

</LinearLayout>
```
###没有问题继续看fragment加载的内容
首先查看navigation是否错误
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/login_navigation"
    app:startDestination="@+id/loginFragment">

    <fragment
        android:id="@+id/loginFragment"
        android:name="com.***.LoginFragment"
        android:label="fragment_login"
        tools:layout="@layout/fragment_login" />
</navigation>
```
再看加载的loginfragment的布局文件

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/white">
```

###找到错误，是之前的databinding最外层layout标签没有删掉

有多级xml加载的情况下，需要逐层找到问题点，不能过于依赖log中的错误定位

个人不推荐在xml中绑定viewmodle的参数和方法
