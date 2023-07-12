# Mavericks: Counter App

开始使用Mavericks很简单。一旦它被[整合到你的项目中](setup.md)，让我们走过一个简单的计数器例子。我们要做这个：

![counter.png](https://upload-images.jianshu.io/upload_images/24860325-6c2ab50367565167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当你点击计数器的文字时，计数就会上升一个。
这个例子的代码可以在[这里](https://github.com/airbnb/mavericks/tree/main/counter)找到。

## 创建计数器状态

创建Mavericks屏幕的第一步是将其作为状态的函数来建模。将屏幕建模为状态的函数是一个有用的概念，因为它是：
1. 易于测试
1. 便于你和其他工程师进行推理
1. 渲染相同，不受导致它的事件顺序的影响
1. 强大到可以渲染任何类型的屏幕

一个计数器屏幕的建模很简单。
```kotlin
data class CounterState(val count: Int = 0) : MavericksState
```
* CounterViewModel将CounterState作为一个构造参数，因为Mavericks为你创建了初始状态。它这样做是为了处理[保存的状态](/saved-state.md)跨进程恢复。
* incrementCount()调用setState，它需要一个类型为`CounterState.() -> CounterState`的还原器。减速器的接收器是运行减速器时的当前状态，它返回更新的状态。`copy`是kotlin数据类的复制函数。

## 创建CounterFragment

首先，我们需要创建布局。它只是一个TextView，将在屏幕中间显示计数。
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/counterText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:textSize="48dp" />
</FrameLayout>
```

然后，我们需要创建我们的Fragment。
```kotlin
class CounterFragment : Fragment(R.layout.counter_fragment), MavericksView {
    private val viewModel: CounterViewModel by fragmentViewModel()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        counterText.setOnClickListener {
            viewModel.incrementCount()
        }
    }

    override fun invalidate() = withState(viewModel) { state ->
        counterText.text = "${state.count}"
    }
}
```
* CounterFragment实现了MavericksView，但你可以选择有一个实现它的基础片段来代替。
* 我们使用Mavericks视图模型委托来获取或创建一个新的ViewModel，这个Fragment的范围。如果不存在，它将被创建，如果存在，则返回现有的，例如，在旋转屏幕之后。
* 我们设置了一个点击监听器，调用`viewModel.incrementCount()`。在你的ViewModel上调用命名的函数，就像MVI世界中的_intent_。
* 我们覆盖了`MavericksView'的`invalidate()'。`invalidate()`将在用视图模型委托检索的任何视图模型的状态改变时被调用。
* 我们添加了`withState(viewModel)`来访问`invalidate()`被调用时我们ViewModel的当前状态。
* 我们将`state.count`渲染到`TextView`的文本中。

***

祝贺你，你已经建立了你的第一个Mavericks屏幕!

# Mavericks: Counter App

Getting started with Mavericks is simple. Once it is [integrated into your project](setup.md), let's walk through a simple counter example. We're going to make this:

![Counter](/images/counter.png)

When you click the counter text, the count goes up by one.
Code for this sample can be found [here](https://github.com/airbnb/mavericks/tree/main/counter).

## Create CounterState

The first step in creating a Mavericks screen is to model it as a function of state. Modeling a screen as a function of state is a useful concept because it is:
1. Easy to test
1. Easy for you and other engineers to reason through
1. Renders the same independently of the order of events leading up to it
1. Powerful enough to render any type of screen

A counter screen is simple to model.
```kotlin
data class CounterState(val count: Int = 0) : MavericksState
```
* CounterState contains a single immutable (`val`) property, count, it is an integer, and it defaults to 0.
* CounterState is a [data class](https://kotlinlang.org/docs/reference/data-classes.html) so that Kotlin creates equals and hashCode automatically.
* CounterState implementation of `MavericksState` signals the intention that this class will be used as Mavericks state.

## Create CounterViewModel

Next, we need to create a ViewModel which will own CounterState and be responsible for updating it.
```kotlin
class CounterViewModel(initialState: CounterState) : MavericksViewModel<CounterState>(initialState) {
    fun incrementCount() {
      setState { copy(count = count + 1) }
    }
}
```
* CounterViewModel takes CounterState as a constructor parameter because Mavericks creates your initial state for you. It does this to handle [saved state](/saved-state.md) across process restoration.
* incrementCount() calls setState which takes a reducer with the type `CounterState.() -> CounterState `. The receiver of the reducer is the current state when the reducer is run and it returns the updated state. `copy` is the kotlin data class copy function.

## Create CounterFragment

First, we need to create the layout. It is simply a TextView that will display the count in the middle of the screen.
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/counterText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:textSize="48dp" />
</FrameLayout>
```

Then, we need to create our Fragment.
```kotlin
class CounterFragment : Fragment(R.layout.counter_fragment), MavericksView {
    private val viewModel: CounterViewModel by fragmentViewModel()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        counterText.setOnClickListener {
            viewModel.incrementCount()
        }
    }

    override fun invalidate() = withState(viewModel) { state ->
        counterText.text = "${state.count}"
    }
}
```
* CounterFragment implements MavericksView but you may choose to have a base fragment that implements it instead.
* We use the Mavericks view model delegates to get or create a new ViewModel scoped to this Fragment. It will be created if one doesn't exist or return the existing one if it does, after rotating the screen, for example.
* We set up a click listener that calls `viewModel.incrementCount()`. Calling named functions on your ViewModel is like the _intent_ from the MVI world.
* We override `invalidate()` from `MavericksView`. `invalidate()` will be called any time the state for any view model retrieved with a view model delegate changes.
* We added `withState(viewModel)` to access the current state of our ViewModel at the time `invalidate()` was called.
* We render `state.count` to the `TextView`'s text.

***

Congratulations, you have built your first Mavericks screen!
