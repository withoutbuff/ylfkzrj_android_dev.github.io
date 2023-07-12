# 线程

在大多数情况下，你从来不需要考虑甚至意识到Mavericks在引擎盖下使用的线程模型。然而，熟悉一下它可能对理解引擎盖下发生的事情有好处。

Mavericks是线程安全的，几乎所有与视图无关的东西都在后台线程上运行。然而，Mavericks将多线程的大部分挑战抽象化了。

状态更新不是同步处理的。它们被放在一个队列中，并在后台线程中运行。
换句话说：
```kotlin
fun setCount() {
//计数为0
setState { copy(count = 1) }
// 计数仍然是0，直到上面的还原器在还原器队列线程上运行。
}
```

为了更容易操作，在ViewModel内部的`withState`调用在`setState`队列被完全刷新后在同一线程上运行。
```kotlin
fun setAndRetrieveState() {
  println('A')
  setState {
    println('B')
    copy(count = 1)
  }
  println('C')
  withState { state ->
    println('D')
  }
  println('E')
}
```
这将打印： [a, c, e, b, d] 。

你可以查看一些更复杂的排序情况[这里]（https://github.com/airbnb/mavericks/blob/release/2.0.0/mvrx/src/test/kotlin/com/airbnb/mvrx/SetStateWithStateOrderingTest.kt）。然而，大多数情况下，它都会像你所期望的那样，你不必考虑线程模型的问题。

然而，有时需要同步获取视图的数据，所以作为一种方便，当从ViewModel外部调用时，`withState`是同步运行的。
```kotlin
fun invalidate() = withState(viewModel) { state ->
// 这个块被立即运行并返回这个lambda的最终表达式。
}
```

默认情况下，Mavericks为处理`withState`/`setState`队列创建了一个共享调度器。你可以通过将其注入`MavericksViewModelConfigFactory.storeContextOverride`来设置你自己的调度器（例如`Dispatchers.Default`）。

# Threading

For the most part, you never have to think about or even be aware of the threading model Mavericks uses under the hood. However, it may be good to familiarize yourself with it to understanding what is happening under the hood.

Mavericks is thread-safe and nearly everything non-view related runs on background threads. However, Mavericks abstracts away most of the challenges of multi-threading.

State updates are not processed synchronously. They are placed on a queue and run on a background thread.
In other words:
```kotlin
fun setCount() {
  // Count is 0
  setState { copy(count = 1) }
  // Count is still 0 until the reducer above is run on the reducer queue thread.
}
```

To make this easier to work with, `withState` calls from within a ViewModel are run on the same thread after the `setState` queue has been fully flushed.
```kotlin
fun setAndRetrieveState() {
  println('A')
  setState {
    println('B')
    copy(count = 1)
  }
  println('C')
  withState { state ->
    println('D')
  }
  println('E')
}
```
This would print: [A, C, E, B, D]

You can view some more complex ordering situations [here](https://github.com/airbnb/mavericks/blob/release/2.0.0/mvrx/src/test/kotlin/com/airbnb/mvrx/SetStateWithStateOrderingTest.kt). However, most of the time it will be have as you would expect and you don't have to think about the threading model.

However, it is sometimes necessary to get data synchronously for views so as a convenience, `withState` _is_ run synchronously when called from outside of the ViewModel.
```kotlin
fun invalidate() = withState(viewModel) { state ->
    // This block is run immediately and returns the final expression of this lambda.
}
```

By default Mavericks creates a shared dispatcher for processing `withState`/`setState` queue. You can set your own dispatcher (e.g. `Dispatchers.Default`) by injecting it into `MavericksViewModelConfigFactory.storeContextOverride`
