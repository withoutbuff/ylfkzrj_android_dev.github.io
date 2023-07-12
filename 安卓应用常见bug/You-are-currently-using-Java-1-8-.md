An exception occurred applying plugin request [id: 'com.android.application'] > Failed to apply plugin 'com.android.internal.application'. > Android Gradle plugin requires Java 11 to run. You are currently using Java 1.8.

###解决方法
1.安装jdk，并且手动配置java home

2.启用安装的jdk
```
For Windows -> File-> Settings-> Build,Execution,Deployment-> Build Tools-> Gradle-> Gradle JDK and select java 11
```

File
Settings
Build，Execution，Deployment
Build Tools 
Gradle
![在设置中更改jdk](https://upload-images.jianshu.io/upload_images/24860325-b744a221caf8595e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


参考链接
[Error: Android Gradle plugin requires Java 11 to run. You are currently using Java 1.8\. -& Failed to apply plugin 'com.android.internal.application' - Stack Overflow](https://stackoverflow.com/questions/68728918/error-android-gradle-plugin-requires-java-11-to-run-you-are-currently-using-ja)
