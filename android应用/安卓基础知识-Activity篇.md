####activity
首先推荐大家看看官方文档
https://developer.android.google.cn/guide/components/activities/intro-activities?hl=zh_cn
官方文档写得非常详细，也非常有逻辑。
这里补充一些个人见解。
#####1.activity是什么？
Android原生开发里用的最多的组件，但不是必须用到的。它的存在，就是给用户一个交互的界面。
在新的官方开发指南中，这个组件逐渐边缘化，官方更推荐使用fragment，减少使用activity。
#####2.我们要怎么用？
简单来讲就是在对应的生命周期函数里，写一些想应的代码，不同的设计架构，可能会有不同的编程思路。MVC模式中，所有的控件变化，业务逻辑，都会写在activity的生命周期函数里。（尤其集中在三个最常用的生命周期函数里。onCreate，onResume，onPause）
```
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
//必须实现的回调函数
//在此处完成初始化，此时avtivity还不能被用户所见。
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val navView: BottomNavigationView = findViewById(R.id.nav_view)

        val navController = findNavController(R.id.nav_host_fragment)
        // Passing each menu ID as a set of Ids because each
        // menu should be considered as top level destinations.
        val appBarConfiguration = AppBarConfiguration(setOf(
                R.id.navigation_home, R.id.navigation_dashboard, R.id.navigation_notifications))
        setupActionBarWithNavController(navController, appBarConfiguration)
        navView.setupWithNavController(navController)
    }

    override fun onStart() {
        super.onStart()
//进入已启动状态
//对用户可见
//一般较少使用
    }

    override fun onResume() {
        super.onResume()
// 开始与用户互动之前调用此回调
//该 Activity 位于 Activity 堆栈的顶部，捕获所有用户输入
//到这里就真正的开始和用户交互了
//大部分核心功能都是在 onResume() 方法中实现的（官方说的，但我经历的项目也好，看过的书也好，基本都写在onCreate（）居多）
    }

    override fun onPause() {
        super.onPause()
//当 Activity 失去焦点并进入“已暂停”状态时，被调用
//启动下一个activity或者按HOME键回到主界面
//在这里一定不能做太耗时操作，比如持久化数据，网络请求
    }

    override fun onStop() {
        super.onStop()
//完全不可见时调用
//如果系统资源不足，杀掉某个activity时，会略过此方法，直抵onDestroy（），所以回收资源也不要写在这里
    }

    override fun onRestart() {
        super.onRestart()
//当处于“已停止”状态的 Activity 即将重启时，调用
//例如，回到上一个activity时，通过“最近使用”回到的页面
    }

    override fun onDestroy() {
        super.onDestroy()
//释放资源。
//比如注销广播接收器，注销绑定的service，
//清除不需要的bitmap，
//注销第三方库eventBus等等。
    }
}
    override fun onRestoreInstanceState(
        savedInstanceState: Bundle?,
        persistentState: PersistableBundle?
    ) {
        super.onRestoreInstanceState(savedInstanceState, persistentState)
//恢复上一次状态
//savedInstanceState会记录一些数据
    }

    override fun onSaveInstanceState(outState: Bundle, outPersistentState: PersistableBundle) {
        super.onSaveInstanceState(outState, outPersistentState)
//保存当前状态
//outState把当前数据保存起来
    }
```
到此出，就可以说会使用activity了，接下来是进一步了解。
#####3.生命周期
官网上的一幅图，可以说非常经典
![生命周期流程图](https://upload-images.jianshu.io/upload_images/24860325-3156c1c97e0c5d7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
了解生命周期的目的，是为了处理更复杂的业务逻辑，一个activity的生命周期不会像一根直线一样，从onCreate到onDestroy，一条路走通关。所以引入生命周期，管理不同activity的状态，让它们有序地进场退场，来满足不同的业务逻辑。
具体详解请看官方文档
https://developer.android.google.cn/guide/components/activities/activity-lifecycle?hl=zh_cn
着重从下面开始看：
Activity 状态和从内存中弹出

说到生命周期，就要谈谈启动模式
四种启动模式，也是管控activity进出用户视线的重要手段。
四种启动模式，可以看作activity和返回栈之间不同的相处模式。
定义启动模式
https://developer.android.google.cn/guide/components/activities/tasks-and-back-stack?hl=zh_cn#TaskLaunchModes
1."standard"（默认模式）
标准的先进后出模式
返回栈会按照先进后出模式，管理每一个activity实例。
2."singleTop"
首先看返回栈顶是否为所需的Activity实例
如果是，则调用此Activity的onNewIntent() 方法来将 intent （可能会带一些参数之类的）转送给该实例
如果不是，就会在返回栈顶新建一个实例。
使用场景：
比如app接收到某个通知，点击通知要跳转到的activity，可以是singleTop模式
3."singleTask"
和"singleTop"相似，都是调用onNewIntent()
不同之处在于，singleTask检测整个返回栈，是否有实例
如果有，则popTo此实例（将此实例之上的其他实例全部出栈）
如果没有，则新建实例。
比如app有一个播放页面，只允许有一个实例
4."singleInstance"
直接新建一个返回栈，包含唯一一个所需的实例
比如app接收到某个通知，点击通知要跳转到的activity，可以是singleInstance模式
