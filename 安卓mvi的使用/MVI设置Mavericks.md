# 设置Mavericks

将Mavericks添加到你的应用程序只需要三个步骤：

### 1\. 添加依赖项

```groovy
dependencies {
  implementation 'com.airbnb.android:mavericks:x.y.z'
}
```
Mavericks的最新版本是[！[Maven Central](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks)

### 2\. 更新你的基础片段/视图
让你的基础Fragment类实现`MavericksView`。
***

`MavericksView`只是一个接口，所以你可以把它和你现有的基础片段组成。

`MavericksView`可以在任何`LifecycleOwner`上实现，所以如果你不想使用Fragment，你可以创建你自己的视图结构。

### 3\. 初始化Mavericks :id=initialize

在你的应用程序onCreate方法中，添加一行：
```kotlin
Mavericks.initialize(this)
```


如果你打算使用[Mavericks Mocking](/mocking.md)，请用这一行代替：
```kotlin
MockableMavericks.initialize(this)
```

如果你打算使用Jetpack Navigation，你也应该遵循[here](/jetpack-navigation.md)的设置步骤。

### [可选] 设置自定义Mavericks视图模型配置

Mavericks允许你覆盖一些配置，这些配置在每次创建新的ViewModel时都会使用。例如，你可以改变状态存储或订阅的默认调度器。请查看[ViewModelConfigFactory](https://github.com/airbnb/mavericks/blob/main/mavericks/src/main/kotlin/com/airbnb/mvrx/MavericksViewModelConfigFactory.kt)的文档以了解更多信息。

**注意：**如果你创建了自己的配置工厂，请确保正确设置`debugMode'，以便运行[debug checks](https://github.com/airbnb/Mavericks/wiki#debug-checks)。

要使用自定义的配置，请在你的`Mavericks.initialize()'调用中添加这个参数：
```kotlin
viewModelConfigFactory = MavericksViewModelConfigFactory(applicationContext)
```
***
# Setting up Mavericks

Adding Mavericks to your app requires just three steps:

### 1. Add The Dependency

```groovy
dependencies {
  implementation 'com.airbnb.android:mavericks:x.y.z'
}
```
The latest version of Mavericks is [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks)


### 2. Update Your Base Fragment/View
Make your base Fragment class implement `MavericksView`.
***

`MavericksView` is just an interface so you can compose it with your existing base Fragment.

`MavericksView` can be implemented on any `LifecycleOwner` so you may create your own view constructs if you don't want to use Fragments.

### 3. Initialize Mavericks :id=initialize

In your Application onCreate method, add a single line:
```kotlin
Mavericks.initialize(this)
```

If you plan to use [Mavericks Mocking](/mocking.md), use this line instead:
```kotlin
MockableMavericks.initialize(this)
```


If you plan on using Jetpack Navigation, you should also follow the setup steps at [here](/jetpack-navigation.md).

### [Optional] Set Custom Mavericks View Model Configuration

Mavericks lets you override some configuration that is used every time a new ViewModel is created. For example, you could change the default dispatcher for the state store or for subscriptions. Check out the docs for [ViewModelConfigFactory](https://github.com/airbnb/mavericks/blob/main/mavericks/src/main/kotlin/com/airbnb/mvrx/MavericksViewModelConfigFactory.kt) for more info.

**Note:** If you create your own config factory, ensure that you set `debugMode` correctly so that the [debug checks](https://github.com/airbnb/Mavericks/wiki#debug-checks) are run.


To use a custom config, add this parameter to your `Mavericks.initialize()` call:
```kotlin
viewModelConfigFactory = MavericksViewModelConfigFactory(applicationContext)
```
***
