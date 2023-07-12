

# What's new in Mavericks 2.0

### MvRx -> Mavericks
在Mavericks 2.0中，你首先会注意到从MvRx到Mavericks的变化。当MvRx在2017年首次建立时，没有[RxJava](https://github.com/ReactiveX/RxJava)几乎不可能建立一个复杂的应用程序。今天，许多应用程序正在过渡到[Kotlin coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html)，许多新的应用程序将根本不需要RxJava。为了使Mavericks现代化，我们用coroutines完全重写了内部结构，并从核心工件中删除了RxJava 2的依赖性。

核心库中的所有API现在都是基于coroutines的。

### API Changes

如果在下一个值发出的时候，前一个实例还没有完成，采取暂停的lambdas的API将自动取消。换句话说，它们的行为类似于[Flow<T>.mapLatest](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/map-latest.html)。

| MvRx 1.x | Mavericks 2.x |
| ------ | --------- |
| <pre lang="kotlin">subscribe(...): Disposable</pre> |<pre lang="json">onEach(...): Job</pre>|
| <pre lang="kotlin">selectSubscribe(<br> stateProp: KProperty1<S, T>,<br> action: (T) -> Unit,<br>): Disposable</pre>Plus multi-prop overloads |<pre lang="kotlin">onEach(<br> stateProp: KProperty1<S, T>,<br> action: suspend (T) -> Unit,<br>): Job</pre>Plus multi-prop overloads |
| <pre lang="kotlin">asyncSubscribe(<br> stateProp: KProperty1&lt;S, Async&lt;T>>,<br> onFail: (Throwable) -> Unit,<br> onSuccess: (T) -> Unit,<br>): Disposable</pre> |<pre lang="kotlin">onAsync(<br> stateProp: KProperty1&lt;S, Async&lt;T>>,<br> onFail: suspend (Throwable) -> Unit,<br> onSuccess: suspend (T) -> Unit,<br>): Job</pre> |
| `execute` is extension on Observable&lt;T>, Single&lt;T>, and Completable| `execute` is extension on Flow&lt;T>, suspend () -> T, and Deferred&lt;T>|
| No equivalent |<pre lang="kotlin">Flow&lt;T>.setOnEach(dispatcher: Dispatcher, reducer: suspend (T) -> Unit): Job</pre> |
| No equivalent |<pre lang="kotlin">ViewModel&lt;S>.stateFlow: Flow&lt;S></pre>|
| No equivalent |<pre lang="kotlin">suspend ViewModel&lt;S>.awaitState(): S</pre>|

### New Functionality

#### Retain Value for execute

Mavericks现在可以在随后的 "加载 "和 "失败 "状态中保留之前的成功值。你可以阅读更多关于它的信息[here](/async?id=retaining-data-across-reloads-with-retainvalue)。


#### `Flow<T>.setOnEach { copy(..) }`。

有时，你不想把数据流映射到`Async<T>`，你只想把数据流映射到一个状态属性。要做到这一点，你可以调用`Flow<T>.setOnEach { copy(..) }`。当你这样做时，Mavericks会订阅该流，每次都会调用你的reducer的每一个项目，然后在ViewModel被清除时取消该工作。

#### MavericksViewModel不再扩展Jetpack ViewModel

MavericksViewModel可以被使用，其行为与过去完全一样。然而，底层实现不再扩展Jetpack自己的ViewModel。相反，它只是一个普通的Kotlin类，被包裹在Jetpack ViewModel的外壳下。这意味着你可以创建你自己的MavericksViewModels的实例，并在标准委托之外使用它们。  

###从1.x升级 :id=upgrading

从MvRx 1.x升级应该相当简单。Mavericks 2.0包括一个`mavericks-rxjava2`工件，它重新添加了所有现有的基于RxJava的API（尽管它们现在包裹了内部的coroutines实现）。

#### 迁移初始化代码

以前，你需要把`debugMode`传入你的`BaseMvRxViewModel`超类。现在，你在Application.onCreate中初始化MvRx一次。要做到这一点，调用`Mavericks.initialize(this)`。因为`debugMode`不再被单独传入每个ViewModel，你甚至不需要一个基础ViewModel类了。你可以简单地每次扩展`BaseMvRxViewModel`，或者如果你不再需要rxjava2的API，可以迁移到只使用`MavericksViewModel`。

#### 更新BaseMvRxFragment

在MvRx 1.x中，你必须让你的基础片段类扩展`BaseMvRxFragment`。现在，你可以让它只实现`MavericksView`。rxjava2构件仍然使用 "BaseMvRxFragment"，但它已被废弃，所有东西都将继续使用 "MavericksView "接口。

#### 更新测试

在MvRx 1.x中，[MvRxTestRule](https://github.com/airbnb/MvRx/blob/1.5.1/testing/src/main/kotlin/com/airbnb/mvrx/test/MvRxTestRule.kt#L31)会将全局RxJava调度器设置为测试的同步（蹦床）调度器。这对于MvRx状态存储的同步行为是必需的。然而，你的测试可能已经隐含地依赖了这种行为。因此，你可能需要创建你自己的RxRule来添加回这个行为。一个这样的RxRule的例子可以在[这里]（https://gist.github.com/gpeal/8b3af17f75d1450b431842bfe05b4d5c）找到。







# What's new in Mavericks 2.0

### MvRx -> Mavericks

The first thing you'll notice with Mavericks 2.0 is the change from MvRx to Mavericks. When MvRx was first built in 2017, it was nearly impossible to build a complex app without [RxJava](https://github.com/ReactiveX/RxJava). Today, many apps are transitioning to [Kotlin coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html) and many new apps will never need RxJava at all. To modernize Mavericks, we completely rewrote the internals with coroutines and removed the RxJava 2 dependency from the core artifact.

All APIs in the core library are now coroutines based.

### API Changes

APIs that take suspending lambdas will auto-cancel the previous instance if it hasn't finished by the time the next value emits. In other words, they behave like [Flow<T>.mapLatest](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/map-latest.html).

| MvRx 1.x | Mavericks 2.x  |
| ------ | --------- |
| <pre lang="kotlin">subscribe(...): Disposable</pre>    |<pre lang="json">onEach(...): Job</pre>|
| <pre lang="kotlin">selectSubscribe(<br>    stateProp: KProperty1<S, T>,<br>    action: (T) -> Unit,<br>): Disposable</pre>Plus multi-prop overloads    |<pre lang="kotlin">onEach(<br>    stateProp: KProperty1<S, T>,<br>    action: suspend (T) -> Unit,<br>): Job</pre>Plus multi-prop overloads |
| <pre lang="kotlin">asyncSubscribe(<br>    stateProp: KProperty1&lt;S, Async&lt;T>>,<br>    onFail: (Throwable) -> Unit,<br>    onSuccess: (T) -> Unit,<br>): Disposable</pre>    |<pre lang="kotlin">onAsync(<br>    stateProp: KProperty1&lt;S, Async&lt;T>>,<br>    onFail: suspend (Throwable) -> Unit,<br>    onSuccess: suspend (T) -> Unit,<br>): Job</pre> |
| `execute` is extension on Observable&lt;T>, Single&lt;T>, and Completable| `execute` is extension on Flow&lt;T>, suspend () -> T, and Deferred&lt;T>|
| No equivalent    |<pre lang="kotlin">Flow&lt;T>.setOnEach(dispatcher: Dispatcher, reducer: suspend (T) -> Unit): Job</pre> |
| No equivalent    |<pre lang="kotlin">ViewModel&lt;S>.stateFlow: Flow&lt;S></pre>|
| No equivalent    |<pre lang="kotlin">suspend ViewModel&lt;S>.awaitState(): S</pre>|

### New Functionality

#### Retain Value for execute

Mavericks can now retain previously successful values across subsequent `Loading` and `Fail` states. You can read more about it [here](/async?id=retaining-data-across-reloads-with-retainvalue).


#### `Flow<T>.setOnEach { copy(...) }`

Sometimes, you don't want to map a stream of data to `Async<T>`, you just want to map a stream of data to a state property. To do that, you can call `Flow<T>.setOnEach { copy(...) }`. When you do that, Mavericks will subscribe to the flow, call your reducer each time with each item, then cancel the job when the ViewModel is cleared.

#### MavericksViewModel no longer extends Jetpack ViewModel

MavericksViewModel can be used and will behave exactly like it used to. However, the underlying implementation no longer extends Jetpack's own ViewModel. Instead, it is just a normal Kotlin class which gets wrapped in a Jetpack ViewModel under the hood. This means that you can create your own instances of MavericksViewModels and use them outside of the standard delegates.   

### Upgrading from 1.x :id=upgrading

Upgrading from MvRx 1.x should be fairly simple. Mavericks 2.0 includes a `mavericks-rxjava2` artifact which adds back all existing RxJava based APIs (although they now wrap the internal coroutines implementation).

#### Migrate initialization code

Previously, you needed to pass `debugMode` into your `BaseMvRxViewModel` super class. Now, you initialize MvRx one time in Application.onCreate. To do so, call `Mavericks.initialize(this)`. Because `debugMode` is no longer passed into each ViewModel individually, you don't even need a base ViewModel class anymore. You can simply extend `BaseMvRxViewModel` each time or migrate to just using `MavericksViewModel` if you no longer need the rxjava2 APIs.

#### Update BaseMvRxFragment

With MvRx 1.x, you had to make your base Fragment class extend `BaseMvRxFragment`. Now, you can make it just implement `MavericksView`. The rxjava2 artifact still ships with `BaseMvRxFragment` but it is deprecated and everything will continue to work with the `MavericksView` interface. 

#### Update tests

In MvRx 1.x, [MvRxTestRule](https://github.com/airbnb/MvRx/blob/1.5.1/testing/src/main/kotlin/com/airbnb/mvrx/test/MvRxTestRule.kt#L31) would set global RxJava schedulers to be synchronous (trampoline) schedulers for tests. This was required for MvRx state stores to behave synchronously. However, your tests may have implicitly relied on this behavior. As a result, you may need to create your own RxRule to add back this behavior. One such example of an RxRule can be found [here](https://gist.github.com/gpeal/8b3af17f75d1450b431842bfe05b4d5c).
