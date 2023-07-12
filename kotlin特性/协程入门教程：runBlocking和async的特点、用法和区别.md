# 前言
在上一篇文章中，我们介绍了如何使用launch函数来启动一个协程，它会在后台运行，不会阻塞主线程。我们用射箭的比喻来形象地解释了协程的特点，即射出去的箭该干啥就干啥，不会等待结果返回。例如：
```kotlin
// 射出一支箭
launch {
    println("Arrow 1 is flying...")
    delay(1000) // 模拟箭飞行的时间
    println("Arrow 1 hits the target!")
}

// 射出第二支箭
launch {
    println("Arrow 2 is flying...")
    delay(2000) // 模拟箭飞行的时间
    println("Arrow 2 hits the target!")
}

// 射出第三支箭
launch {
    println("Arrow 3 is flying...")
    delay(3000) // 模拟箭飞行的时间
    println("Arrow 3 hits the target!")
}

println("All arrows are shot!")
```

输出结果如下：

```
All arrows are shot!
Arrow 1 is flying...
Arrow 2 is flying...
Arrow 3 is flying...
Arrow 1 hits the target!
Arrow 2 hits the target!
Arrow 3 hits the target!
```

可以看到，主线程在射出所有的箭后就结束了，不会等待任何一个箭的结果。而每个箭都是一个协程，它们在后台并发地执行，按照自己的速度完成任务。

但是有时候，我们可能需要获取协程的结果，或者等待协程完成后再执行其他操作。这时候，我们就需要用到另外两种启动协程的API：runBlocking和async。
|  | 特点 | 用法 | 区别 |
| --- | --- | --- | --- |
| runBlocking | 阻塞当前线程，等待所有协程完成 | 在普通函数中调用协程函数，或者返回一个值 | 会阻塞线程执行，会等待子协程执行完成 |
| async | 不阻塞当前线程，但挂起后续代码，直到调用await方法获取结果 | 在一个协程中等待另一个协程的结果，或者同时启动多个协程并发地执行 | 不会阻塞线程执行，不会等待子协程执行完成 |
# runBlocking
runBlocking函数可以创建一个阻塞当前线程的协程，并等待它及其所有子协程完成后再恢复线程执行。它可以用来连接协程和线程之间的桥梁，让我们可以在普通函数中调用协程函数。例如：

```kotlin
fun main() {
    // 在主线程中启动一个阻塞协程
    runBlocking {
        println("runBlocking start")
        // 在阻塞协程中启动一个子协程
        launch {
            println("launch start")
            delay(1000) // 模拟耗时操作
            println("launch end")
        }
        println("runBlocking end")
    }
    println("main end")
}
```

输出结果如下：

```
runBlocking start
launch start
runBlocking end
launch end
main end
```
![runBlocking](https://upload-images.jianshu.io/upload_images/24860325-d1b6c770f893cada.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，主线程在进入runBlocking后被阻塞了，直到runBlocking内部的子协程完成后才继续执行。而子协程仍然是非阻塞的，它会在后台运行，并在延迟后结束。

另外，runBlocking还可以返回一个值，这个值就是它内部最后一行代码的结果。例如：

```kotlin
fun main() {
    // 在主线程中启动一个阻塞协程，并获取它的返回值
    val result = runBlocking {
        println("runBlocking start")
        // 在阻塞协程中启动一个子协程，并获取它的返回值
        val result = launch {
            println("launch start")
            delay(1000) // 模拟耗时操作
            println("launch end")
            "Hello" // 返回一个字符串值
        }
        println("runBlocking end")
        result // 返回子协程的返回值
    }
    println("main end, result is $result")
}
```

输出结果如下：

```
runBlocking start
launch start
runBlocking end
launch end
main end, result is Hello
```
![runBlocking](https://upload-images.jianshu.io/upload_images/24860325-47d1d13407833df1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，主线程在获取到runBlocking的返回值后才结束，而这个返回值就是子协程的返回值。

# async
async函数可以创建一个可以返回结果的协程，它不会阻塞当前线程，但会挂起后续代码直到调用await方法获取结果。它可以用来实现协程之间的依赖关系，让我们可以在一个协程中等待另一个协程的结果。例如：

```kotlin
fun main() {
    // 在主线程中启动一个协程
    GlobalScope.launch {
        println("launch start")
        // 在协程中启动一个可以返回结果的协程，并获取它的返回值
        val result = async {
            println("async start")
            delay(1000) // 模拟耗时操作
            println("async end")
            "World" // 返回一个字符串值
        }.await() // 调用await方法获取结果，会挂起当前协程直到结果返回
        println("launch end, result is $result")
    }
    println("main end")
}
```

输出结果如下：

```
main end
launch start
async start
async end
launch end, result is World
```
![async](https://upload-images.jianshu.io/upload_images/24860325-eecdc8545cbbc5e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，主线程在启动协程后就结束了，不会等待任何结果。而协程在启动async后会挂起，直到async返回结果后才继续执行。

另外，async还可以同时启动多个协程，并发地执行它们，然后等待所有的结果返回。这样可以提高效率，避免串行地等待每个协程。例如：

```kotlin
fun main() {
    // 在主线程中启动一个协程
    GlobalScope.launch {
        println("launch start")
        // 在协程中同时启动三个可以返回结果的协程，并分别获取它们的返回值
        val result1 = async {
            println("async 1 start")
            delay(1000) // 模拟耗时操作
            println("async 1 end")
            "Hello" // 返回一个字符串值
        }
        val result2 = async {
            println("async 2 start")
            delay(2000) // 模拟耗时操作
            println("async 2 end")
            "World" // 返回一个字符串值
        }
        val result3 = async {
            println("async 3 start")
            delay(3000) // 模拟耗时操作
            println("async 3 end")
            "!" // 返回一个字符串值
        }
        // 调用await方法获取结果，会挂起当前协程直到所有结果返回
        val finalResult = result1.await() + " " + result2.await() + result3.await()
        println("launch end, final result is $finalResult")
    }
    println("main end")
}
```

输出结果如下：

```
main end
launch start
async 1 start
async 2 start
async 3 start
async 1 end
async 2 end
async 3 end
launch end, final result is Hello World!
```
![async](https://upload-images.jianshu.io/upload_images/24860325-f7fdcfb33b6cb4b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，三个async都是并发地执行的，它们的执行时间取决于各自的延迟时间。而协程在等待所有的结果返回后才继续执行。

# 钓鱼模型

为了更形象地理解async启动的协程，我们可以用钓鱼的比喻来解释它们。我们可以把每个Deferred对象（即async返回的对象）看作是一根鱼竿，它有一个await方法来抬杆收鱼。而每个鱼竿都有一个钓鱼时间，就是它内部代码执行的时间。例如：

```kotlin
fun main() {
    // 在主线程中启动一个协程
    GlobalScope.launch {
        println("开始钓鱼")
        // 在协程中同时启动三个可以返回结果的协程，并分别获取它们的返回值
        val rod1 = async {
            println("鱼竿1下水")
            delay(1000) // 模拟钓鱼时间
            println("鱼竿1上钩")
            "鲫鱼" // 返回一个字符串值
        }
        val rod2 = async {
            println("鱼竿2下水")
            delay(2000) // 模拟钓鱼时间
            println("鱼竿2上钩")
            "草鱼" // 返回一个字符串值
        }
        val rod3 = async {
            println("鱼竿3下水")
            delay(3000) // 模拟钓鱼时间
            println("鱼竿3上钩")
            "鲤鱼" // 返回一个字符串值
        }
        // 调用await方法获取结果，会挂起当前协程直到所有结果返回
        val fish1 = rod1.await() // 抬杆收鲫鱼
        val fish2 = rod2.await() // 抬杆收草鱼
        val fish3 = rod3.await() // 抬杆收鲤鱼
        println("结束钓鱼，收获了$fish1, $fish2, $fish3")
    }
    println("主线程结束")
}
```

输出结果如下：

```
主线程结束
开始钓鱼
鱼竿1下水
鱼竿2下水
鱼竿3下水
鱼竿1上钩
鱼竿2上钩
鱼竿3上钩
结束钓鱼，收获了鲫鱼, 草鱼, 鲤鱼
```

可以看到，三个async都是并发地执行的，它们的执行时间取决于各自的延迟时间。而协程在等待所有的结果返回后才继续执行。

# 阻塞探究

我们已经知道了await方法会挂起当前协程，直到获取结果为止。但是这里的挂起并不是阻塞，它只是暂停了当前协程的执行，而不是阻塞了当前线程。这意味着当前线程可以继续做其他事情，而不是等待协程的结果。例如：

```kotlin
fun main() {
    // 在主线程中启动一个协程
    GlobalScope.launch {
        println("launch start")
        // 在协程中同时启动两个可以返回结果的协程，并分别获取它们的返回值
        val result1 = async {
            println("async 1 start")
            delay(1000) // 模拟耗时操作
            println("async 1 end")
            "Hello" // 返回一个字符串值
        }
        val result2 = async {
            println("async 2 start")
            delay(2000) // 模拟耗时操作
            println("async 2 end")
            "World" // 返回一个字符串值
        }
        // 调用await方法获取结果，会挂起当前协程直到结果返回
        val finalResult = result1.await() + " " + result2.await()
        println("launch end, final result is $finalResult")
    }
    // 在主线程中循环打印点号
    repeat(10) {
        print(".")
        Thread.sleep(500) // 模拟耗时操作
    }
    println()
    println("main end")
}
```

输出结果如下：

```
main end
launch start
async 1 start
async 2 start
....async 1 end
....async 2 end
launch end, final result is Hello World
..
main end
```
![async await方法](https://upload-images.jianshu.io/upload_images/24860325-b79ef19bb28a2900.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，主线程在启动协程后并没有被阻塞，而是继续循环打印点号。而协程在等待两个async的结果时也没有阻塞线程，而是让出了线程资源给主线程。当两个async都返回结果后，协程才恢复执行，并打印出最终结果。

# 总结

runBlocking和async都是启动协程的API，它们有以下优缺点和适用场景：

- runBlocking可以创建一个阻塞当前线程的协程，并等待它及其所有子协程完成后再恢复线程执行。它可以用来连接协程和线程之间的桥梁，让我们可以在普通函数中调用协程函数。它还可以返回一个值，这个值就是它内部最后一行代码的结果。
- runBlocking的缺点是它会阻塞当前线程，导致线程资源浪费。它不适合在高并发的场景中使用，因为它会降低系统的吞吐量和响应速度。
- async可以创建一个可以返回结果的协程，它不会阻塞当前线程，但会挂起后续代码直到调用await方法获取结果。它可以用来实现协程之间的依赖关系，让我们可以在一个协程中等待另一个协程的结果。它还可以同时启动多个协程，并发地执行它们，然后等待所有的结果返回。这样可以提高效率，避免串行地等待每个协程。
- async的缺点是它会增加代码的复杂度，因为它需要我们手动管理每个协程的返回值和异常。它还需要我们注意协程的作用域和生命周期，避免内存泄漏和僵尸协程的产生。
- async适合在需要获取协程结果或者需要并发执行多个协程的场景中使用，因为它可以提高代码的可读性和性能。
