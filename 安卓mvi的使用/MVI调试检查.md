# 调试检查

Mavericks有一个调试模式，可以启用一些验证检查。这些检查将防止一些常见的错误或其他问题，这些问题可能会给现实世界的用户带来罕见的、难以调试的问题。

这些检查包括：

* 运行所有的状态还原器两次，确保产生的状态是相同的。这可以确保还原器是纯洁的。
* 所有的状态属性都是不可变的vals而不是vars。
* 状态类型不是以下之一： ArrayList, SparseArray, LongSparseArray, SparseArrayCompat, ArrayMap, and HashMap.
* 状态类的可见性是公开的，所以它可以被创建和恢复。
* 一旦你的State在ViewModel上被设置，它的每个实例就不会被改变。

### 启用调试模式

当你[将Mavericks集成到你的应用程序中](/setup)时，有一些重载可以传入`Context`或明确设置`debugMode`。如果你用`Context`初始化Mavericks（推荐），那么如果你的应用程序是可调试的（`context.applicationInfo.flags`包含`ApplicationInfo.FLAG_DEBUGGABLE`），调试模式将被自动启用。

### 避免在`withState`/`setState`中的缓慢操作
由于所有的`withState`/`setState`块都是按顺序处理的，所以强烈建议避免在这些块内进行慢速调用（阻塞的IO/cpu消耗调用）。你可以使用Android [StrictMode](https://developer.android.com/reference/android/os/StrictMode)来检测此类调用。StrictMode.ThreadPolicy可以被注入到`MavericksViewModelConfigFactory.storeContextOverride`：
```
val threadPolicy = StrictMode.ThreadPolicy.Builder()
    .detectNetwork()
    .penaltyDialog()
    .build()

Mavericks.viewModelConfigFactory = MavericksViewModelConfigFactory(
    this,
    storeContextOverride = if (isDebuggable()) threadPolicy.asContextElement() else EmptyCoroutineContext
)
```

# Debug Checks

Mavericks has a debug mode that enables some validation checks. These checks will safeguard against a number of common mistakes or other issues that could cause rare and hard to debug issues for real world users.

The checks include:

* Running all state reducers twice and ensuring that the resulting state is the same. This ensures that reducers are pure.
* All state properties are immutable vals not vars.
* State types are not one of: ArrayList, SparseArray, LongSparseArray, SparseArrayCompat, ArrayMap, and HashMap.
* State class visibility is public so it can be created and restored
* Each instance of your State is not mutated once it is set on the ViewModel

### Enabling debug mode

When you [integrate Mavericks into your app](/setup), there are overloads to pass in `Context` or to explicitly set `debugMode`. If you initialize Mavericks with `Context` (recommended) then debug mode will automatically be enabled if your application is debuggable (`context.applicationInfo.flags` contains `ApplicationInfo.FLAG_DEBUGGABLE`).


### Avoid slow operations in `withState`/`setState`
As all `withState`/`setState` blocks are processed sequentially, it's highly recommended to avoid slow calls inside these blocks (blocking IO/cpu consuming calls). You can use Android [StrictMode](https://developer.android.com/reference/android/os/StrictMode) to detect such calls. StrictMode.ThreadPolicy can be injected into `MavericksViewModelConfigFactory.storeContextOverride`:
```
val threadPolicy = StrictMode.ThreadPolicy.Builder()
    .detectNetwork()
    .penaltyDialog()
    .build()

Mavericks.viewModelConfigFactory = MavericksViewModelConfigFactory(
    this,
    storeContextOverride = if (isDebuggable()) threadPolicy.asContextElement() else EmptyCoroutineContext
)
```
