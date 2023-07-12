
# Async
Mavericks让处理异步请求变得简单，比如从网络、数据库或其他任何异步的地方获取。Mavericks包括`Async<T>`。Async是一个[密封类](https://kotlinlang.org/docs/reference/sealed-classes.html)，有四个子类：

下面是一个简略的形式：
```kotlin
sealed class Async<out T>(private val value: T?) {

    open operator fun invoke(): T? = value

    object Uninitialized : Async<Nothing>(value = null)

    data class Loading<out T>(private val value: T? = null) : Async<T>(value = value)

    data class Success<out T>(private val value: T) : Async<T>(value = value) {
        override operator fun invoke(): T = value
    }

    data class Fail<out T>(val error: Throwable, private val value: T? = null) : Async<T>(value = value)
}
```

你可以直接调用一个Async对象，如果是 "成功"，它将返回值，否则返回null。
比如说
```kotlin
val foo = Success(5)
println(foo()) // 5
```

### 使用Async进行异步操作（网络、数据库等）与`execute`。
`MavericksViewModel`在常见的异步类型上提供了`execute(...)`扩展，如`suspend () -> T`、`Flow<T>`和`Deferred<T>`。`mvrx-rxjava2`工件有对`Observable<T>`、`Single<T>`和`Completable<T>`的扩展。

当你对这些类型之一调用`execute'时，它将开始执行，立即发出`Loading'，然后在成功时发出`Success'或`Fail'，发出一个新值，或失败。

Mavericks会在ViewModel的`onCleared`中自动处理订阅，所以你永远不需要管理生命周期或取消订阅。

对于它发出的每一个事件，它将调用`reducer`，它获取当前状态并返回一个更新的状态，就像`setState`一样。

```kotlin
val foo = Success(5)
println(foo()) // 5
```

### 使用Async进行异步操作（网络、数据库等）与`execute`。
`MavericksViewModel`在常见的异步类型上提供了`execute(...)`扩展，如`suspend () -> T`、`Flow<T>`和`Deferred<T>`。`mvrx-rxjava2`工件有对`Observable<T>`、`Single<T>`和`Completable<T>`的扩展。

当你对这些类型之一调用`execute'时，它将开始执行，立即发出`Loading'，然后在成功时发出`Success'或`Fail'，发出一个新值，或失败。

Mavericks会在ViewModel的`onCleared`中自动处理订阅，所以你永远不需要管理生命周期或取消订阅。

对于它发出的每一个事件，它都会调用`reducer`，它获取当前状态并返回一个更新的状态，就像`setState`一样。

执行一个网络请求看起来像：
```kotlin
interface WeatherApi {
    suspend fun fetchTemperature()： Int
}

//在你的ViewModel中的一个函数里面。
suspend {
    weatherApi.fetchTemperature()
}.execute { copy(currentTemperature = it) }
```

或者对于一个[Kotlin Flow](https://kotlinlang.org/docs/reference/coroutines/flow.html)：
```kotlin
interface WeatherRepository {
    fun fetchTemperature()： 流程<Int>。
}

//在你的ViewModel中的一个函数里面。
weatherRepository.fetchTemperature().execute { copy(currentTemperature = it) }
```

在这种情况下：
* `currentTemperature`是`Async<Int>`类型，最初设置为`Uninitialized`。
* 调用`执行'后，`currentTemperature'被设置为`Loading()`。
* 如果API调用成功, `currentTemperature`被设置为`Success(temp)`.
* 如果API调用失败，`currentTemperature`被设置为`Fail(e)`。
* 如果ViewModel在`fetchTemperature()`完成之前被清除，API请求将被取消。

###订阅异步属性
如果你的状态属性是`Async`，你可以使用`onAsync`而不是`onEach`来订阅状态变化。

你可以像这样使用它：
```kotlin
data class MyState(val name: Async<String>) ： MavericksState
...
onAsync(MyState::name) { name ->
// 当名字是成功的时候被调用，以及任何时候它的变化。
}

// 或者如果你想处理失败的情况
onAsync(
MyState::name、
onFail = { e -> .... },
onSuccess = { name -> ... }
)
```

###在不同的调度器上执行
你可能想在一个不同的调度器上运行你的暂停函数。要做到这一点，请将一个`Dispatcher'作为第一个参数传给`execute()'：
```kotlin
suspend {
    weatherApi.fetchTemperature()
}.execute(Dispatchers.IO) { copy(currentTemperature = it) }
```

###用`retainValue`在重新加载时保留数据
你可能有一个模型，你想刷新数据，除了刷新的加载/失败状态外，还要显示最后的成功数据。要做到这一点，使用可选的`retainValue`参数为`execute`，MvRx将自动把存储在该属性中的值持续到随后的`Loading`或`Fail`状态。

```kotlin
suspend {
weatherApi.fetchTemperature()
}.execute(retainValue = MyState::currentTemperature) { copy(currentTemperature = it) }
```
在前面的例子中，如果你在数据为`Success(5)`时再次调用`fetchData()`，随后的值将是：
* `正在加载(5)'。
* `成功(6)'。

你的用户界面可以检查`data is Loading`来决定是否显示一个加载指示器，然而在新数据加载时调用`data()`来渲染最新的值。
你也可以使用`viewModel.onAsync`与`onFail`块，在刷新失败时显示一个提示条或错误信息，而不必拿走你已经显示的第一组数据。



# Async&lt;T&gt;
Mavericks makes dealing with async requests like fetching from a network, database, or anything else asynchronous easy. Mavericks includes `Async<T>`. Async is a [sealed class](https://kotlinlang.org/docs/reference/sealed-classes.html) with four subclasses:

Here is an abridged form:
```kotlin
sealed class Async<out T>(private val value: T?) {

    open operator fun invoke(): T? = value

    object Uninitialized : Async<Nothing>(value = null)

    data class Loading<out T>(private val value: T? = null) : Async<T>(value = value)

    data class Success<out T>(private val value: T) : Async<T>(value = value) {
        override operator fun invoke(): T = value
    }

    data class Fail<out T>(val error: Throwable, private val value: T? = null) : Async<T>(value = value)
}
```

You can directly invoke an Async object and it will return either the value if it is `Success` or null otherwise.
For example:
```kotlin
val foo = Success(5)
println(foo()) // 5
```

### Using Async for asynchronous actions (network, db, etc.) with `execute`
`MavericksViewModel` ships an `execute(...)` extension on common asynchronous types such as `suspend () -> T`, `Flow<T>`, and `Deferred<T>`. The `mvrx-rxjava2` artifact has extensions for `Observable<T>`, `Single<T>`, and `Completable<T>`.

When you call `execute` on one of these types, it will begin executing it, immediately emit `Loading`, and then emit `Success` or `Fail` when it succeeds, emits a new value, or fails.

Mavericks will automatically dispose of the subscription in `onCleared` of the ViewModel so you never have to manage the lifecycle or unsubscribing.

For each event it emits, it will call the `reducer` which takes the current state and returns an updated state just like `setState`

Executing a network request looks like:
```kotlin
interface WeatherApi {
    suspend fun fetchTemperature(): Int
}

// Inside of a function in your ViewModel.
suspend {
    weatherApi.fetchTemperature()
}.execute { copy(currentTemperature = it) }
```

Or for a [Kotlin Flow](https://kotlinlang.org/docs/reference/coroutines/flow.html):
```kotlin
interface WeatherRepository {
    fun fetchTemperature(): Flow<Int>
}

// Inside of a function in your ViewModel.
weatherRepository.fetchTemperature().execute { copy(currentTemperature = it) }
```

In this case:
* `currentTemperature` is of type `Async<Int>` and originally set to `Uninitialized`
* After calling `execute`, `currentTemperature` is set to `Loading()`
* If the API call succeeds, `currentTemperature` is set to `Success(temp)`
* If the API call fails, `currentTemperature` is set to `Fail(e)`
* If the ViewModel is cleared before `fetchTemperature()` completes, the API request is cancelled

### Subscribing to Async properties
If your state property is `Async`, you can use `onAsync` instead of `onEach` to subscribe to state changes.

You use it like:
```kotlin
data class MyState(val name: Async<String>) : MavericksState
...
onAsync(MyState::name) { name ->
    // Called when name is Success and any time it changes.
}

// Or if you want to handle failures
onAsync(
    MyState::name,
    onFail = { e -> .... },
    onSuccess = { name -> ... }
)
```

### Executing on a different dispatcher
You may want to run your suspend function on a different dispatcher. To do that, pass a `Dispatcher` as the first parameter to `execute()`:
```kotlin
suspend {
    weatherApi.fetchTemperature()
}.execute(Dispatchers.IO) { copy(currentTemperature = it) }
```

### Retaining data across reloads with `retainValue`
You may have a model where you want to refresh data and show the last successful data in addition to the loading/failure state of the refresh. To do this, use the optional `retainValue` parameter for `execute` and MvRx will automatically persist the value stored in that property to subsequent `Loading` or `Fail` states.

```kotlin
suspend {
    weatherApi.fetchTemperature()
}.execute(retainValue = MyState::currentTemperature) { copy(currentTemperature = it) }
```
In the previous example, if you called `fetchData()` again when data is `Success(5)`, the subsequent values will be:
* `Loading(5)`
* `Success(6)`

Your UI can check for `data is Loading` to determine whether to show a loading indicator yet call `data()` to render the most recent value while the new data loads.
You can also use `viewModel.onAsync` with on `onFail` block to show a snackbar or error message when the refresh failed without having to take away the first set of data that you already displayed.
