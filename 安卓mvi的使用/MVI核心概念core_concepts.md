## 核心概念
掌握Mavericks只需要使用三个类：`MavericksState`，`MavericksViewModel`，和`MavericksView`。

## MavericksState
创建Mavericks屏幕的第一步是将其作为状态的函数来建模。MavericksState接口[不做任何事情](https://github.com/airbnb/MvRx/blob/main/mavericks/src/main/kotlin/com/airbnb/mvrx/MavericksState.kt)本身，但表明了你的类被用作状态的意图。

将屏幕建模为状态的函数是一个有用的概念，因为它是：
1. 线程安全
1. 便于你和其他工程师进行推理
1. 渲染是相同的，与导致它的事件的顺序无关
1. 强大到可以渲染任何类型的屏幕
1. 易于测试

Mavericks还将强制要求你的状态类：
1. 是一个kotlindata class
1. 只使用不可变的属性
1. 每个属性都有默认值，以确保你的屏幕可以立即被渲染。

Mavericks通过其[debug checks]（debug-checks.md）来执行这些规定。

这个概念使得推理和测试一个屏幕变得非常容易，因为给定一个状态类，你可以有很高的信心，你的屏幕看起来是正确的。
例子
```kotlin
data class UserState(
    val score: Int = 0,
    val previousHighScore: Int = 150,
    val livesLeft: Int = 99,
) : MavericksState
```

#### 派生属性 :id=derived
因为state只是一个普通的Kotlindata class，所以你可以创建派生属性来表示特定的状态条件，比如这样：
```kotlin
data class UserState(
    val score: Int = 0,
    val previousHighScore: Int = 150,
    val livesLeft: Int = 99,
) : MavericksState {
    // Properties inside the body of your state class are "derived".
    val pointsUntilHighScore = (previousHighScore - score).coerceAtLeast(0)
    val isHighScore = score >= previousHighScore
}
```
将逻辑放在像`pointsUntilHighScore'或`isHighScore'这样的派生属性中，意味着：
1. 你可以使用`onEach'来订阅这些派生属性的变化。
1. 它将永远不会与底层状态不同步
1. 它非常容易推理
1. 编写单元测试是非常容易的

## MavericksViewModel

一个ViewModel负责：
1. 更新状态
2. 2.为其他类提供一个状态流，供其订阅（MavericksViewModel.stateFlow）。

Mavericks ViewModels在概念上与[Jetpack ViewModels](https://developer.android.com/topic/libraries/architecture/viewmodel)几乎相同，只是在MavericksState类上增加了通用性。

###更新状态
在viewModel中，你调用`setState { copy(yourProp = newValue) }`。如果对这个语法不熟悉的话：
1. lambda的签名是`S.() -> S`，意思是当lambda被调用时，lambda的接收方（又称`this`）是当前的状态，它返回新的lambda
1. `copy`来自于状态类是Kotlin的[data class](https://kotlinlang.org/docs/reference/data-classes.html)
1. 该lambda不是同步执行的。它被放在一个队列中，并在后台线程中运行。更多信息见[threading](threading.md)

### 处理异步/数据库/网络操作
轻松处理异步操作是Mavericks的主要目标之一。查看 "Async<T>"和 "execute(..) "的[the docs](async.md)来了解更多。

### 订阅状态变化
你可以在你的ViewModel中订阅状态变化。例如，你可能想这样做来进行分析。这通常在`init { ... }`块中完成。

```kotlin
//每次状态改变时调用
onEach { state ->
}
```

```kotlin
// 只在propA发生变化时调用。
onEach(YourState::propA) { a ->
}
// 只在propA、propB或propC发生变化时调用。
onEach(YourState::propA, YourState::propB, YourState::propC) { a, b, c ->
}
```

**提示：**如果你在一个`onEach'块中调用`setState'，考虑使用一个[派生属性](#derived)。

### stateFlow
`MavericksViewModel`暴露了一个`stateFlow`属性，它是一个普通的Kotlin Flow，可以发出当前的状态以及任何未来的更新，你可以随意使用。像上面的`onEach`这样的助手只是围绕它的包装，具有自动取消生命周期的功能。

###访问一次状态
如果你只想检索一次状态的值，你可以使用`withState { state -> ... }`.

当从ViewModel内部调用时，这将不会被同步运行。它将被放在一个后台队列中，以便所有待定的`setState`还原器在你的`withState`调用之前被调用。
## MavericksView
`MavericksView`是你实际将你的状态类渲染到屏幕上的地方。大多数情况下，这将是一个Fragment，但它不一定是。
通过实现`MavericksView'，你可以：
1. 你可以通过任何一个视图模型委托来访问`MavericksViewModel`。这样做会自动订阅变化并调用`invalidate()`。
1. 覆盖`invalidate()`函数。当上述委托访问的任何视图模型的状态发生变化时，它将被调用。`invalidate()`被用来在每次状态变化时重绘用户界面。

#### ViewModel委托
1. `activityViewModel()`将ViewModel的范围扩大到Activity。在Activity中的所有请求ViewModel类型的片段都将收到相同的实例。对于在屏幕之间共享数据很有用。
1. `fragmentViewModel()`将ViewModel的作用范围扩大到片段。它将被子片段访问，但父片段或兄弟片段将得到一个不同的实例。
1. `parentFragmentViewModel()`在父片段树上行走，直到找到一个已经创建了所需类型的ViewModel。如果没有找到，它将自动创建一个范围在最高的父片段。
1. `existingViewModel()`与`activityViewModel()`相同，只是如果ViewModel不是由其他人创建的，则会抛出。
1. `navGraphViewModel(navGraphId: Int)`将ViewModel的作用范围扩大到具有该ID的[Jetpack Navigation](https://developer.android.com/guide/navigation)图。这需要mvrx-navigation构件（[docs](jetpack-navigation.md)）。

如果你想要多个相同类型的ViewModels，你可以在任何一个委托中传递一个`keyFactory`。

#### 手动订阅状态
大多数时候，重写`invalidate()`并更新你的视图就足够了。然而，如果你想订阅状态来做一些事情，比如启动动画，你可以调用ViewModel上的任何`onEach`订阅。如果你的视图是一个片段，这些订阅应该在`onCreate()`中设置。

#### 访问一次状态
如果你只想检索一次状态的值，你可以使用`withState { state -> ... }`.

当从ViewModel外部调用时，这将被同步运行。

#### 触发状态变化
视图模型应该暴露出可以被视图调用的命名函数。例如，一个计数器的视图模型可以暴露一个函数`incrementCount()`来创建一个清晰的API，供视图访问。

通过www.DeepL.com/Translator（免费版）翻译

通过www.DeepL.com/Translator（免费版）翻译
## Core Concepts
Mastering Mavericks only requires using three classes: `MavericksState`, `MavericksViewModel`, and `MavericksView`.

## MavericksState
The first step in creating a Mavericks screen is to model it as a function of state. The MavericksState interface [doesn't do anything](https://github.com/airbnb/MvRx/blob/main/mavericks/src/main/kotlin/com/airbnb/mvrx/MavericksState.kt) itself but signals the intention of your class to be used as state.

Modeling a screen as a function of state is a useful concept because it is:
1. Thread safe
1. Easy for you and other engineers to reason through
1. Renders the same independently of the order of events leading up to it
1. Powerful enough to render any type of screen
1. Easy to test

Mavericks will also enforce that your state class:
1. Is a kotlin data class
1. Uses only immutable properties
1. Has default values for every property to ensure that your screen can be rendered immediately

Mavericks enforces these through its [debug checks](debug-checks.md)

This concept makes reasoning about and testing a screen trivially easy because given a state class, you can have high confidence that your screen will look correct.
Example
```kotlin
data class UserState(
    val score: Int = 0,
    val previousHighScore: Int = 150,
    val livesLeft: Int = 99,
) : MavericksState
```

#### Derived Properties :id=derived
Because state is just a normal Kotlin data class, you can create derived properties to represent specific state conditions like this:
```kotlin
data class UserState(
    val score: Int = 0,
    val previousHighScore: Int = 150,
    val livesLeft: Int = 99,
) : MavericksState {
    // Properties inside the body of your state class are "derived".
    val pointsUntilHighScore = (previousHighScore - score).coerceAtLeast(0)
    val isHighScore = score >= previousHighScore
}
```
Placing logic as a derived property like `pointsUntilHighScore` or `isHighScore` means that:
1. You can subscribe to changes to just these derived properties using `onEach`.
1. It will _never_ be out of sync with the underlying state
1. It is extremely easy to reason about
1. It is extremely easy to write unit tests for

## MavericksViewModel

A ViewModel is responsible for:
1. Updating state
2. Exposing a stream of states for other classes to subscribe to (MavericksViewModel.stateFlow)

Mavericks ViewModels are conceptually nearly identical to [Jetpack ViewModels](https://developer.android.com/topic/libraries/architecture/viewmodel) with the addition of being generic on a MavericksState class.

### Updating state
From within a viewModel, you call `setState { copy(yourProp = newValue) }`. If this syntax is unfamiliar:
1. The signature of the lambda is `S.() -> S` meaning the receiver (aka `this`) of the lambda is the current state when the lambda is invoked and it returns the new lambda
1. `copy` comes from the fact that the state class is a Kotlin [data class](https://kotlinlang.org/docs/reference/data-classes.html)
1. The lambda is _not_ executed synchronously. It is put on a queue and run on a background thread. See [threading](threading.md) for more information

### Handling async/db/network operations
Handling asynchronous operations with ease was one of the primary goals of Mavericks. Check out [the docs](async.md) for `Async<T>` and `execute(...)` to learn more.

### Subscribing to state changes
You can subscribe to state changes in your ViewModel. You may want to do this for analytics, for example. This usually done in the `init { ... }` block.

```kotlin
// Invoked every time state changes
onEach { state ->
}
```

```kotlin
// Invoked whenever propA changes only.
onEach(YourState::propA) { a ->
}
// Invoked whenever propA, propB, or propC changes only.
onEach(YourState::propA, YourState::propB, YourState::propC) { a, b, c ->
}
```

**TIP:** If you are calling `setState` from within an `onEach` block, consider using a [derived property](#derived).

### stateFlow
`MavericksViewModel` exposes a `stateFlow` property which is a normal Kotlin Flow that emits the current state as well as any future updates and can be used however you would like. Helpers such as `onEach` above are just wrappers around it with automatic lifecycle cancellation.

### Accessing state once
If you just want to retrieve the value of state one time, you can use `withState { state -> ... }`.

When called from within a ViewModel, this will _not_ be run synchronously. It will be placed on a background queue so that all pending `setState` reducers are called prior to your `withState` call.

## MavericksView
`MavericksView` is where you actually render your state class to the screen. Most of the time, this will be a Fragment but it doesn't have to be.
By implementing `MavericksView`, you:
1. You can get access to a `MavericksViewModel` via any of the view model delegates. Doing so will automatically subscribe to changes and call `invalidate()`.
1. Override the `invalidate()` function. It is called any time the state for any view model accessed by the above delegates changes. `invalidate()` is used to redraw the UI on each state change

#### ViewModel delegates
1. `activityViewModel()` scopes the ViewModel to the Activity. All Fragments within the Activity that request a ViewModel of this type will receive the same instance. Useful for sharing data between screens.
1. `fragmentViewModel()` scopes the ViewModel to the Fragment. It will be accessible to children fragments but parent or sibling fragments would get a different instance.
1. `parentFragmentViewModel()` walks up the parent fragment tree until it finds one that has already created a ViewModel of the desired type. If none is found, it will automatically create one scoped to the highest parent fragment.
1. `existingViewModel()` Same as `activityViewModel()` except it throws if the ViewModel was not already created by somebody else.
1. `navGraphViewModel(navGraphId: Int)` scopes the ViewModel to the [Jetpack Navigation](https://developer.android.com/guide/navigation) graph with that id. This requires the mvrx-navigation artifact ([docs](jetpack-navigation.md)).

If you want multiple ViewModels of the same type, you can pass a `keyFactory` into any of the delegates.

#### Subscribing to state manually
Most of the time, overriding `invalidate()` and updating your views is sufficient. However, if you want to subscribe to state to do things like start animations, you may call any of the `onEach` subscriptions on your ViewModel. If your view is a Fragment, these subscriptions should be set up in `onCreate()`.

#### Accessing state once
If you just want to retrieve the value of state one time, you can use `withState { state -> ... }`.

When called from outside a ViewModel, this will be run synchronously.

#### Triggering state changes
The ViewModel should expose named functions that can be called by the view. For example, a counter view model could expose a function `incrementCount()` to create a clear API accessible to the view.
