# 使用launch函数创建和运行协程


## launch启动协程

要创建一个协程，我们可以使用launch函数，它是Kotlin标准库中提供的一个顶层函数，它接收一个lambda表达式作为参数，表示要在协程中执行的代码块。launch函数会返回一个Job对象，表示协程的任务。例如，下面的代码创建了一个协程，并在其中打印了一条消息：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个协程
    val job = launch {
        // 在协程中执行的代码
        println("Hello, coroutine!")
    }
}
```

launch函数会立即返回，不会等待协程中的代码执行完毕。这意味着主线程会继续执行后面的代码，而不会被阻塞。因此，如果我们在上面的代码后面添加一条打印语句：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个协程
    val job = launch {
        // 在协程中执行的代码
        println("Hello, coroutine!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
}
```

那么输出可能是这样的：

```
Hello, main thread!
Hello, coroutine!
```

也就是说，主线程先于协程打印了消息。当然，输出也可能是相反的顺序，这取决于协程和主线程的调度情况。但无论如何，我们可以看到，协程的执行不会影响主线程的执行。

## 非阻塞

上面的例子中，我们没有指定协程运行在哪个线程上。实际上，launch函数会根据当前的上下文（CoroutineContext）来决定使用哪个线程来执行协程。如果没有指定上下文，默认情况下，launch函数会使用Dispatchers.Default作为上下文，它表示使用一个共享的线程池来运行协程。这样可以避免创建过多的线程，节省资源。

我们可以使用第一个参数来指定launch函数使用哪个上下文来运行协程。例如，我们可以使用Dispatchers.Main来指定在主线程上运行协程：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个在主线程上运行的协程
    val job = launch(Dispatchers.Main) {
        // 在协程中执行的代码
        println("Hello, coroutine on main thread!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
}
```

这样，输出就一定是这样的：

```
Hello, main thread!
Hello, coroutine on main thread!
```

因为两个打印语句都在同一个线程上执行。

但是，在主线程上运行协程并不意味着会阻塞主线程。协程仍然可以使用suspend关键字来标记可以暂停和恢复的函数，从而实现异步和非阻塞的操作。例如，我们可以使用delay函数来模拟一个耗时的操作：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个在主线程上运行的协程
    val job = launch(Dispatchers.Main) {
        // 在协程中执行的代码
        println("Hello, coroutine on main thread!")
        // 模拟一个耗时2秒的操作
        delay(2000)
        println("Bye, coroutine on main thread!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
    // 模拟一个耗时1秒的操作
    Thread.sleep(1000)
    println("Bye, main thread!")
}
```

这样，输出就是这样的：

```
Hello, main thread!
Hello, coroutine on main thread!
Bye, main thread!
Bye, coroutine on main thread!
```

我们可以看到，当协程遇到delay函数时，它会暂停自己的执行，让出线程给其他任务。这样，主线程就可以继续执行后面的代码，而不会被阻塞。当delay函数结束后，协程会恢复自己的执行，继续打印消息。

## 协程和线程的关系

从上面的例子中，我们可以看出，协程和线程有一定的关系，但也有很大的区别。协程是一种运行在线程上的任务，它可以在同一个线程上并发执行多个协程，也可以在不同的线程上切换执行协程。线程是一种操作系统提供的资源，它可以并行执行多个任务，但是创建和销毁线程需要消耗资源。

协程和线程之间有一个重要的联系，就是当主线程销毁时，所有运行在该线程上的协程也会停止执行。这是因为协程是依赖于线程存在的，如果没有线程来调度协程，那么协程就无法运行。因此，如果我们想要让协程执行完毕，我们需要保证主线程不会提前退出。例如，在上面的例子中，如果我们去掉Thread.sleep(1000)这一行代码：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个在主线程上运行的协程
    val job = launch(Dispatchers.Main) {
        // 在协程中执行的代码
        println("Hello, coroutine on main thread!")
        // 模拟一个耗时2秒的操作
        delay(2000)
        println("Bye, coroutine on main thread!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
    // 不再让主线程等待
}
```

那么输出可能是这样的：

```
Hello, main thread!
Hello, coroutine on main thread!
```

也就是说，当主线程执行完毕后，它就退出了，导致运行在该线程上的协程也无法继续执行。如果我们想要让协程执行完毕，我们需要让主线程等待协程结束。有两种方法可以实现这一点：

- 一种是使用job对象的join方法，它会挂起当前线程，直到协程结束为止。例如：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 创建一个在主线程上运行的协程
    val job = launch(Dispatchers.Main) {
        // 在协程中执行的代码
        println("Hello, coroutine on main thread!")
        // 模拟一个耗时2秒的操作
        delay(2000)
        println("Bye, coroutine on main thread!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
    // 等待协程结束
    job.join()
    println("Bye, main thread!")
}
```

- 另一种是使用runBlocking函数，它会创建一个新的协程，并阻塞当前线程，直到协程结束为止。例如：

```kotlin
// 导入kotlinx.coroutines包
import kotlinx.coroutines.*

fun main() {
    // 使用runBlocking函数创建一个协程
    runBlocking {
        // 在协程中执行的代码
        println("Hello, coroutine on main thread!")
        // 模拟一个耗时2秒的操作
        delay(2000)
        println("Bye, coroutine on main thread!")
    }
    // 在主线程中执行的代码
    println("Hello, main thread!")
}
```

这样，输出就是这样的：

```
Hello, coroutine on main thread!
Bye, coroutine on main thread!
Hello, main thread!
```

我们可以看到，主线程等待了协程执行完毕后，才继续执行后面的代码。

## 射箭模型

launch函数创建的协程有一个特点，就是它不会返回任何结果。这就像射出去的箭，不会返回任何信息，只能执行一次。这种协程适合执行不需要结果的任务，例如打印消息，更新UI，发送网络请求等。如果我们想要从协程中获取结果，我们需要使用另一种函数，叫做async函数，它会返回一个Deferred对象，表示一个延迟计算的结果。我们可以使用await方法来获取Deferred对象的结果，或者使用其他方法来处理Deferred对象。关于async函数和Deferred对象的用法，我们将在下一篇文章中介绍。

## launch方法定义

最后，我们来看一下launch函数的定义：

```kotlin
fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job
```
![launch函数的参数和返回值](https://upload-images.jianshu.io/upload_images/24860325-fb8dcbab4a5ae117.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

launch函数有三个参数：

- context：表示协程的上下文，用于指定协程运行在哪个线程上，以及其他一些属性。默认值是EmptyCoroutineContext，表示使用当前的上下文。
- start：表示协程的启动模式，用于控制协程何时开始执行。默认值是CoroutineStart.DEFAULT，表示立即启动协程。其他的启动模式有CoroutineStart.LAZY（表示懒加载协程），CoroutineStart.ATOMIC（表示原子启动协程），CoroutineStart.UNDISPATCHED（表示不调度启动协程）。
- block：表示要在协程中执行的代码块，它是一个lambda表达式，接收一个CoroutineScope对象作为接收者（this），并且可以使用suspend关键字来标记可以暂停和恢复的函数。

launch函数返回一个Job对象，表示协程的任务。Job对象有一些方法和属性，用于控制和监视协程的状态。例如：

- cancel()：取消协程的执行。
- join()：等待协程结束。
- isActive：表示协程是否处于活跃状态。
- isCancelled：表示协程是否被取消。
- isCompleted：表示协程是否已完成。

launch函数是定义在CoroutineScope接口中的一个扩展函数。CoroutineScope接口表示一个协程的作用域，它包含了一个CoroutineContext对象，用于存储协程的上下文信息。CoroutineScope接口有一些实现类和工厂方法，用于创建不同类型的作用域。例如：
![CoroutineScope接口的实现类和工厂方法](https://upload-images.jianshu.io/upload_images/24860325-a105c03349009fb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- GlobalScope：表示全局作用域，它是一个单例对象，用于创建不受限制的协程。
- CoroutineScope()：表示通用作用域，它是一个工厂方法，接收一个CoroutineContext对象作为参数，用于创建自定义的作用域。
- MainScope()：表示主作用域，它是一个工厂方法，返回一个使用Dispatchers.Main作为上下文的作用域。
- runBlocking()：表示阻塞作用域，它是一个函数，接收一个lambda表达式作为参数，用于创建一个阻塞当前线程的作用域。

## 总结

本文介绍了使用launch函数来创建和运行协程，以及协程的特点和与线程的关系，帮助读者理解协程的概念和用法。协程是一种轻量级的并发编程技术，它可以让我们在同一个线程上执行多个任务，提高程序的效率和响应性。协程的思想是将一个复杂的任务分解成多个小的步骤，每个步骤可以暂停和恢复执行，从而实现异步和非阻塞的操作。要使用协程，我们需要了解如何创建和启动一个协程，以及协程的特点和与线程的关系。launch函数是一种创建协程的方法，它接收一个lambda表达式作为参数，表示要在协程中执行的代码块。launch函数会立即返回，不会等待协程中的代码执行完毕。这意味着主线程会继续执行后面的代码，而不会被阻塞。协程可以使用suspend关键字来标记可以暂停和恢复的函数，从而实现异步和非阻塞的操作。协程是运行在线程上的任务，它可以在同一个线程上并发执行多个协程，也可以在不同的线程上切换执行协程。当主线程销毁时，所有运行在该线程上的协程也会停止执行。因此，如果我们想要让协程执行完毕，我们需要保证主线程不会提前退出。launch函数创建的协程有一个特点，就是它不会返回任何结果。这就像射出去的箭，不会返回任何信息，只能执行一次。这种协程适合执行不需要结果的任务，例如打印消息，更新UI，发送网络请求等。如果我们想要从协程中获取结果，我们需要使用另一种函数，叫做async函数，它会返回一个Deferred对象，表示一个延迟计算的结果。关于async函数和Deferred对象的用法，我们将在下一篇文章中介绍。
