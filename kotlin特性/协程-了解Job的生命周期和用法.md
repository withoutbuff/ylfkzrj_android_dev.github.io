# 协程 了解Job的生命周期和用法

## 前言

在前面的文章中，我们学习了协程的基本概念，以及如何使用launch和async函数来创建和启动协程。但是，创建和启动协程并不是协程的全部，我们还需要知道如何监测和操控协程的状态和执行流程。这就是本文要介绍的内容，即协程的Job的概念，以及如何通过Job来管理协程。

## Job和协程的关系

首先，我们来通过一个比喻，来说明Job和协程的关系。假设你要去旅行，你需要订一张机票。你可以通过网上订票系统来完成这个任务。当你提交了你的信息和支付方式后，网上订票系统会给你返回一个订单号，这个订单号就相当于一个Job。通过这个订单号，你可以查询你的机票状态，比如是否已经出票，是否可以改签或退票等。如果你改变了主意或者有其他原因，你也可以通过这个订单号来取消你的机票。

同样地，当我们创建一个协程时，我们也会得到一个Job对象。这个Job对象就代表了这个协程的任务。通过这个Job对象，我们可以查询协程的状态，比如是否正在运行，是否已经取消或完成等。我们也可以通过这个Job对象来取消协程，如果我们不想让它继续执行或者有其他原因。

![Job和协程的关系](https://upload-images.jianshu.io/upload_images/24860325-240520f088f34ee1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## Job的作用

那么，Job到底是什么呢？官方文档给出了这样的定义：

> A background job. Conceptually, a job is a cancellable thing with a life-cycle that culminates in its completion.

翻译过来就是：

> 一个后台任务。从概念上讲，一个任务是一个可取消的东西，它有一个以其完成为终点的生命周期。

从这个定义中，我们可以看出Job可以做两件事情：

- 监测协程的状态
- 操控协程的执行

下面我们分别来看看这两件事情。

## Job的状态

Job有三个外部状态属性，分别是：

- isActive: 表示协程是否正在运行
- isCancelled: 表示协程是否已经被取消
- isCompleted: 表示协程是否已经完成

注意：这三个属性并不是互斥的，它们可能同时为true或false。比如，在协程被取消后，它可能还在运行一段时间（直到遇到取消检查点），此时isActive和isCancelled都为true；在协程完成后，它可能是正常完成也可能是异常完成（被取消或抛出异常），此时isCompleted和isCancelled都为true。

| 状态| isActive | isCancelled | isCompleted |
| --------- | -------- | ----------- | ----------- |
| New       | false    | false       | false       |
| Active    | true     | false       | false       |
| Completing| true     | false       | false       |
| Cancelling| false    | true        | false       |
| Cancelled | false    | true        | true        |
| Completed | false    | false       | true        |

我们可以通过这三个属性来判断协程的状态，并根据不同的状态做出相应的处理。比如：

```kotlin
// 创建一个协程
val job = GlobalScope.launch {
    // 模拟一个耗时操作
    delay(1000)
    println("Hello, world!")
}

// 在主线程中检查协程的状态
println("isActive: ${job.isActive}") // true
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // false

// 等待一段时间
Thread.sleep(1500)

// 再次检查协程的状态
println("isActive: ${job.isActive}") // false
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // true
```

输出结果：

```
isActive: true
isCancelled: false
isCompleted: false
Hello, world!
isActive: false
isCancelled: false
isCompleted: true
```

## Job的方法

Job有两个方法，分别是：

- start(): 启动协程
- cancel(): 取消协程

这两个方法可以影响协程的状态，从而影响协程的执行流程。我们来看看它们的用法和效果。

### start()

start()方法用于启动协程。通常情况下，我们不需要显式地调用这个方法，因为当我们使用launch或async函数创建协程时，它们会自动调用这个方法。但是，如果我们想要延迟启动协程，或者手动控制协程的启动时机，我们可以使用CoroutineStart.LAZY参数来创建协程，并在需要的时候调用start()方法。比如：

```kotlin
// 创建一个延迟启动的协程
val job = GlobalScope.launch(start = CoroutineStart.LAZY) {
    // 模拟一个耗时操作
    delay(1000)
    println("Hello, world!")
}

// 在主线程中检查协程的状态
println("isActive: ${job.isActive}") // false
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // false

// 启动协程
job.start()

// 再次检查协程的状态
println("isActive: ${job.isActive}") // true
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // false

// 等待一段时间
Thread.sleep(1500)

// 再次检查协程的状态
println("isActive: ${job.isActive}") // false
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // true
```

输出结果：

```
isActive: false
isCancelled: false
isCompleted: false
isActive: true
isCancelled: false
isCompleted: false
Hello, world!
isActive: false
isCancelled: false
isCompleted: true
```

### cancel()

cancel()方法用于取消协程。当我们调用这个方法时，它会将协程的状态标记为取消状态，并尝试停止协程的执行。但是，这并不意味着协程会立即停止，因为协程可能还在运行一些不可取消的代码，或者没有遇到取消检查点。取消检查点是指一些可以响应取消请求的函数或语句，比如delay(), yield(), withContext(), withTimeout()等。只有当协程遇到了取消检查点，它才会真正地停止执行，并抛出一个CancellationException异常。

我们可以通过cancel()方法来终止一个不想要或者出错的协程。比如：

```kotlin
// 创建一个协程
val job = GlobalScope.launch {
    // 模拟一个耗时操作
    delay(1000)
    println("Hello, world!")
}

// 在主线程中检查协程的状态
println("isActive: ${job.isActive}") // true
println("isCancelled: ${job.isCancelled}") // false
println("isCompleted: ${job.isCompleted}") // false

// 取消协程
job.cancel()

// 再次检查协程的状态
println("isActive: ${job.isActive}") // false
println("isCancelled: ${job.isCancelled}") // true
println("isCompleted: ${job.isCompleted}") // false

// 等待一段时间
Thread.sleep(1500)

// 再次检查协程的状态
println("isActive: ${job.isActive}") // false
println("isCancelled: ${job.isCancelled}") // true
println("isCompleted: ${job.isCompleted}") // true

```

输出结果：

```
isActive: true
isCancelled: false
isCompleted: false
isActive: false
isCancelled: true
isCompleted: false

Exception in thread "DefaultDispatcher-worker-1" kotlinx.coroutines.JobCancellationException: StandaloneCoroutine was cancelled; job=StandaloneCoroutine{Cancelling}@6d311334

isActive: false
isCancelled: true
isCompleted: true

```

## Job的内部机制

Job有一个内部状态机制，用于表示和维护协程的生命周期。



Job的内部状态机制有以下几种状态：

- New: 协程刚刚被创建，还没有启动。
- Active: 协程已经启动，正在运行。
- Completing: 协程已经完成了它的主体逻辑，但是还有一些子协程没有完成。
- Cancelling: 协程已经被取消，但是还有一些子协程或者不可取消的代码没有完成。
- Cancelled: 协程已经被取消，并且没有任何子协程或者不可取消的代码在运行。
- Completed: 协程已经正常完成，并且没有任何子协程在运行。

这些状态之间的转换条件和含义如下：

- New -> Active: 当协程被启动时，它会从New状态转换为Active状态。这个转换可以由start()方法或者CoroutineStart.LAZY参数触发。
- Active -> Completing: 当协程完成了它的主体逻辑时，它会从Active状态转换为Completing状态。这个转换可以由return语句或者异常抛出触发。
- Active -> Cancelling: 当协程被取消时，它会从Active状态转换为Cancelling状态。这个转换可以由cancel()方法或者超时函数触发。
- Completing -> Completed: 当协程没有任何子协程在运行时，它会从Completing状态转换为Completed状态。这个转换表示协程正常完成了它的任务。
- Cancelling -> Cancelled: 当协程没有任何子协程或者不可取消的代码在运行时，它会从Cancelling状态转换为Cancelled状态。这个转换表示协程异常完成了它的任务，并且抛出了一个CancellationException异常。
- Cancelling -> Completed: 当协程被取消时，如果它没有遇到任何取消检查点，并且正常完成了它的任务，它会从Cancelling状态转换为Completed状态。这个转换表示协程虽然被取消了，但是仍然执行了它的逻辑，并且返回了一个结果。

我们可以通过下面的示意图来理解这些状态之间的转换：

![状态之间的转换](https://upload-images.jianshu.io/upload_images/24860325-59de6b6f6c6ddc43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 等待和监听协程

除了查询和控制协程的状态外，我们还可以通过两个方法来等待或监听协程的完成事件，以便在协程结束后做一些后续处理。这两个方法分别是：

- join(): 等待协程完成
- invokeOnCompletion(): 监听协程完成

### join()

join()方法用于等待协程完成。当我们调用这个方法时，当前线程会被阻塞，直到目标协程结束。我们可以通过这个方法来同步协程和线程之间的执行流程。比如：

```kotlin
// 创建一个协程
val job = GlobalScope.launch {
    // 模拟一个耗时操作
    delay(1000)
    println("Hello, world!")
}

// 在主线程中等待协程完成
println("Before join")
job.join()
println("After join")
```

输出结果：

```
Before join
Hello, world!
After join
```
![ join()](https://upload-images.jianshu.io/upload_images/24860325-e8b06b3e40b80f51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### invokeOnCompletion()

invokeOnCompletion()方法用于监听协程完成。当我们调用这个方法时，我们需要传入一个回调函数，这个回调函数会在目标协程结束时被调用。我们可以通过这个方法来异步地处理协程的完成事件，比如打印日志，释放资源，处理异常等。比如：

```kotlin
// 创建一个协程
val job = GlobalScope.launch {
    // 模拟一个耗时操作
    delay(1000)
    println("Hello, world!")
}

// 在主线程中监听协程完成
job.invokeOnCompletion {
    println("Job is done")
}
```

输出结果：

```
Hello, world!
Job is done
```

## Deferred接口

Deferred接口是Job的子接口，它提供了一个额外的功能，就是等待协程返回结果。当我们使用async函数创建协程时，我们会得到一个Deferred对象，这个对象有一个await()函数，用于等待协程的结果。我们可以通过这个函数来获取协程的返回值，并根据不同的值做出相应的处理。比如：

```kotlin
// 创建一个返回结果的协程
val deferred = GlobalScope.async {
    // 模拟一个耗时操作
    delay(1000)
    // 返回一个随机数
    (1..10).random()
}

// 在主线程中等待协程的结果
val result = deferred.await()
println("Result is $result")
```

输出结果：

```
Result is 7
```


## 总结

本文介绍了协程的Job的概念，以及如何通过Job监测和操控协程的状态和执行流程。我们学习了Job的三个状态属性，两个方法，以及它的内部状态机制。我们还学习了如何等待或监听协程的完成事件，以及如何获取协程的返回结果。通过这些知识，我们可以更好地管理和控制协程的生命周期，从而提高协程的可靠性和效率。
