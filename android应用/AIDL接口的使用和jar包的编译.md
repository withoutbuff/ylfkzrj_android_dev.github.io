发现网上这类文章比较少，之前自己做的时候遇到了很多问题，现在就新版本的as（2021.1.1 patch3）做一个从开发到提供jar包的全流程教程，欢迎转载，转载前请留言。
#一.什么是AIDL
>参考官方文章
Android 接口定义语言 (AIDL)
https://developer.android.google.cn/guide/components/aidl?hl=zh_cn

简单讲就是c/s模式的跨进程通信手段，一般用于framework中的java service与其他进程通信的手段，也会用于app开发中，向其他app提供接口。
#二.为什么使用AIDL
和其他跨进程通信方式之间的优缺点
|名称| 优点 | 缺点 |适用场景 |
| :------------ | :-------------: | :-------------: |:-------------: |
|Bundle|	简单易用	|只能传输Bundle支持的数据类型	|四大组件间的进程通信
|文件共享|	简单易用|	不适合高并发场景，无法及时通信|	无高并发访问，交换简单数据，实时性不高|
|AIDL	|功能强大，支持一对多并发通信，支持及时通信|	使用复杂，需要处理线程同步	|一对多且有远程调用需求|
|Messenger|	功能一般，支持一对多串行，支持实时通信|	不能很好处理高并发，不支持远程调用，数据只能使用Message传输|	低并发的一对多即时通信，无须直接返回结果，无远程调用|
|ContentProvider|	数据访问方面功能强大，支持一对多并发数据共享|	受约束的AIDL，主要提供数据的增删改查|	一对多进程间数据共享|
|Socket	|功能强大，支持一对多并发实时通信	|实现复杂，不支持直接的远程调用|	网络数据交互|
注：该表格信息来自https://www.jianshu.com/p/cb8fa31a459f

#三.使用的方式
关于创建接口，传递数据的方式，第一章中的官方文档讲的非常详细了，我这里做一个整体项目的详细讲解。
在as中，从创建项目，到交付jar包的整个流程
##1.创建AIDL项目（项目架构简介和操作步骤）
###**（1）**和普通项目一样，首先new 一个project
![new project.png](https://upload-images.jianshu.io/upload_images/24860325-fdcfe2cdfe35b6d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###**（2）**在本例中，我们在同一个project中建立两个app和一个module
顺序不能变，从上到下有依赖
**1.new一个module，作为AIDL库**
![new module.png](https://upload-images.jianshu.io/upload_images/24860325-e174f9df9360e8f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![new aidl module.png](https://upload-images.jianshu.io/upload_images/24860325-c7dac29960af7874.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![new aidl文件.png](https://upload-images.jianshu.io/upload_images/24860325-56b377671a2a5c5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

新建两个aidl文件，一个作为callback，一个作为service
module详细结构
![aidl module结构.png](https://upload-images.jianshu.io/upload_images/24860325-5bc79d239c65f18d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**2.new一个app，作为服务端**
![new module.png](https://upload-images.jianshu.io/upload_images/24860325-e174f9df9360e8f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![选phone&table.png](https://upload-images.jianshu.io/upload_images/24860325-2af78b3f49d0e85b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
新建一个activity（占位用的，不然安装不了）和一个service（核心类，是远程服务端）
![aidlservice.png](https://upload-images.jianshu.io/upload_images/24860325-60ee99afb262a71f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**3.默认的app，用来做客户端**
app总览如下图
![app总览.png](https://upload-images.jianshu.io/upload_images/24860325-58a522b39d4a7543.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如下图，一个activity
包含按钮：
①绑定/解绑连接

②调用远程方法

③加入/离开客户端列表（列表由远程服务维护）

④获取客户端列表，注册/解注册回调


UI如下
![按钮.png](https://upload-images.jianshu.io/upload_images/24860325-d509977d823c340a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)








##2.完善AIDL文件
```
// IParticipateCallback.aidl
package com.mingtianyeshihaotianqi.aidlservicelib;

interface IParticipateCallback {
    // 用户加入或者离开的回调
    void onParticipate(String name, boolean joinOrLeave);
}
```
里面的方法对应五种操作
```
// IRemoteService.aidl
package com.mingtianyeshihaotianqi.aidlservicelib;

import com.mingtianyeshihaotianqi.aidlservicelib.IParticipateCallback;

interface IRemoteService {
    int someOperate(int a, int b);

    void join(IBinder token, String name);
    void leave(IBinder token);
    List<String> getParticipators();

    void registerParticipateCallback(IParticipateCallback cb);
    void unregisterParticipateCallback(IParticipateCallback cb);
}

```
写完记得make一下，生成中间java类
![make module.png](https://upload-images.jianshu.io/upload_images/24860325-e30cff41dbb591d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##3.完善remote service
首先继承AIDL编译出的中间类IRemoteService.Stub，并覆写里面的方法，在对应的方法中实现相应的逻辑。
```
    private RemoteCallbackList<IParticipateCallback> mCallbacks = new RemoteCallbackList<>();//维护一个客户端回调列表

    private final IRemoteService.Stub mBinder = new IRemoteService.Stub() {//实现一个stub，这里的方法会被客户端调用。
        ...
        @Override
        public void registerParticipateCallback(IParticipateCallback cb) throws RemoteException {
            mCallbacks.register(cb);//在这里实现注册回调的逻辑
        }

        @Override
        public void unregisterParticipateCallback(IParticipateCallback cb) throws RemoteException {
            mCallbacks.unregister(cb);
        }

        @Override
        public void join(IBinder token, String name) throws RemoteException {
            ...
            // 通知client加入
            notifyParticipate(client.mName, true);
        }

        @Override
        public void leave(IBinder token) throws RemoteException {
            ...
            // 通知client离开
            notifyParticipate(client.mName, false);
        }
    };
```
一个小细节，在销毁service时释放所有的回调
```
    @Override
    public void onDestroy() {
        super.onDestroy();

        // 取消掉所有的回调
        mCallbacks.kill();
    }
```
##4.完善客户端app
首先导入aidl依赖

![project structure.png](https://upload-images.jianshu.io/upload_images/24860325-4cc5a0f659801395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![add module2.png](https://upload-images.jianshu.io/upload_images/24860325-7a4b2084a0b038dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![add module3.png](https://upload-images.jianshu.io/upload_images/24860325-4e3676d1a8ded93e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

继承连接匿名内部类
```
    private ServiceConnection mServiceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            Toast.makeText(MainActivity.this, "Service connected", Toast.LENGTH_SHORT).show();

            mService = IRemoteService.Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            Toast.makeText(MainActivity.this, "Service disconnected", Toast.LENGTH_SHORT).show();

            mService = null;
        }
    };
```
实现回调接口，监听remote service的变化
```
    private IParticipateCallback mParticipateCallback = new IParticipateCallback.Stub() {

        @Override
        public void onParticipate(String name, boolean joinOrLeave) throws RemoteException {
            if (joinOrLeave) {
                mAdapter.add(name);
            } else {
                mAdapter.remove(name);
            }
        }
    };
```

下面是官网中的启动方法
```
Intent intent = new Intent(Binding.this, RemoteService.class);
            intent.setAction(IRemoteService.class.getName());
            bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
            intent.setAction(ISecondary.class.getName());
            bindService(intent, secondaryConnection, Context.BIND_AUTO_CREATE);
            isBound = true;
            callbackText.setText("Binding.");
```
但是请注意，在android11系统中，对读取使用其他应用包名做了限制，所以需要在清单文件中增加对应的包名请求，不然会报错找不到服务
```
<queries>
        <package android:name="com.mingtianyeshihaotianqi.aidlservice"/>
    </queries>
```
##5.使用manager类在jar包中提供绑定逻辑
这里其实是把之前导入module依赖，转换成楼使用jar包依赖，一般开发出的aidl接口，多以jar包形式提供给其他同事使用，所以编译jar包也是必须的技能

#四.注意事项
##1.处理客户端异常退出
在remoteService中继承IBinder.DeathRecipient，并覆写binderDied，在此方法中处理异常。
```
    private final class Client implements IBinder.DeathRecipient {
        public final IBinder mToken;
        public final String mName;

        public Client(IBinder token, String name) {
            mToken = token;
            mName = name;
        }

        @Override
        public void binderDied() {
            // 客户端死掉，执行此回调
            int index = mClients.indexOf(this);
            if (index < 0) {
                return;
            }

            Log.d(TAG, "client died: " + mName);
            mClients.remove(this);

            // 通知client离开
            notifyParticipate(mName, false);
        }
    }
```
##2.处理服务端异常退出
调用您在接口上定义的方法。您应始终捕获 `[DeadObjectException](https://developer.android.google.cn/reference/android/os/DeadObjectException?hl=zh-cn)` 异常，系统会在连接中断时抛出此异常。您还应捕获 `[SecurityException](https://developer.android.google.cn/reference/java/lang/SecurityException?hl=zh-cn)` 异常，当 IPC 方法调用中两个进程的 AIDL 定义发生冲突时，系统会抛出此异常。

>参考文章
Android Service实现双向通信
https://www.race604.com/communicate-with-remote-service-1/
https://www.race604.com/communicate-with-remote-service-2/
https://www.race604.com/communicate-with-remote-service-3/
远程Service中的DeathRecipient和RemoteCallbackList
https://www.jianshu.com/p/5a2fd8946d1b
Android DeadObjectException 异常 aidl通信
https://blog.csdn.net/jqwei2/article/details/98611551
