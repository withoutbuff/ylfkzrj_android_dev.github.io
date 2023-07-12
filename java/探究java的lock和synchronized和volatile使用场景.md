#一、lock、synchronized和volatile是什么
##1.lock是java中的一个接口
我们常用的是它的子类ReentrantLock，ReentrantReadWriteLock，ReadWriteLock
![lock子类.png](https://upload-images.jianshu.io/upload_images/24860325-5f91267d90b44b6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
使用灵活，可以自行判断需要加什么样的锁（主要看业务逻辑读取写入的频率和重要性来决定），升级降级
但需要手动处理加锁，解锁
##2.synchronized是java中的关键字
自动处理加锁解锁
可用于变量，方法，类对象
使用简单方便，但是不灵活（比如跨方法就不太好用）
##3.volatile是java中的关键字
提供轻量级的同步机制
因为volatile保证可见性，并禁止指令重排，**不保证原子性**（a++之类的语句属于非原子操作，多线程修改同一变量会出错），所以**不能保证线程安全**，但在**多线程读取，单线程写入**的场景下是可以使用的(主要考虑不会引起线程上下文切换，性能开销较小)。
#二、怎么使用
##1.volatile关键字
###（1）应用于原子操作
**三种原子操作**
1.基本类型赋值，等式右边不能是变量
 a=1;可以 
 b=a;不可以
（有说法称，long和double是64位数据，在32位环境下不属于原子操作）
2.引用reference的赋值操作
```
        int[] arr1 = { 1, 2, 3}; 
        int[] arr2 = arr1;  //**在这属于引用赋值**
//特点是修改arr1中的值，arr2也跟着改变
//因为两个栈变量指向同一个堆变量

        arr2[0] = 10; //改变第二个数组的第一个元素
        System.out.println(arr1[0]); //结果为10
        System.out.println(arr2[0]); //结果为10
```
3.java.concurrent.Atomic.*包中所有类的一切操作
![atomic包.png](https://upload-images.jianshu.io/upload_images/24860325-3f4ed73faca5a4a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**1.状态标志位**
```
    volatile boolean isDone = false;//赋值属于原子操作

    private void receiver(){
        while (!isDone){
            //例如，监听网络数据包
        }
    }
```
**2.独立观察位**
```
public class CustomLinkedList {
    public volatile Node lastNode;
    
    public void add(Node node){
        lastNode = node;//引用赋值原子操作
    }
}
```

###（2）在读取场景替换Synchronized
```
public class ResponseCache {
    private volatile int pageIndex;

    public int getPageIndex() {
        return pageIndex;
    }//这里少用一次synchronized 

    public synchronized void setPageIndex(int pageIndex) {
        this.pageIndex = pageIndex;
    }
}
```
###（3）单例中使用其禁止重排特性
**双重检查锁定**
```
public class CustomPlayer {
    private static CustomPlayer singleton;
    private CustomPlayer(){

    }

    public static CustomPlayer getInstance(){
        if(singleton == null){
            synchronized (CustomPlayer.class){
                if(singleton == null){
                    singleton = new CustomPlayer();
                    //此处有指令重排序隐患
                }
            }
        }
        return singleton;
    }
}
```
创建类实例的步骤：(new CustomPlayer())

1、memory = allocate() 分配对象内存空间

2、ctorInstance() 初始化对象

3、instance = memory 指针重定向
由于JVM和CPU优化，多线程环境下可能会发生指令重排序，顺序变为1、3、2。此时对象还没有完成初始化，可能就会出现一个线程检测到instance不为空，而直接获取instance去使用对象导致错误。

**使用volatile禁止重排序**
```
public class CustomPlayer {
    private volatile static CustomPlayer singleton;
     //唯一的变化，增加volatile
    private CustomPlayer(){
        
    }
    
    public static CustomPlayer getInstance(){
        if(singleton == null){
            synchronized (CustomPlayer.class){
                if(singleton == null){
                    singleton = new CustomPlayer();
                    //volatile禁止“初始化对象”和”设置singleton指向内存空间”的重排序
                }
            }
        }
        return singleton;
    }
}
```
##2.synchronized关键字
（1）修饰实例方法
作用于当前对象实例
```
synchronized void method() {
  //业务代码
}
```
（2）修饰静态方法
作用于类
```
synchronized void staic method() {
  //业务代码
}
```
（3）修饰代码块
作用于当前对象实例
```
synchronized(this) {
  //业务代码
}
synchronized(this.class) {
  //业务代码
}

class service{
  String lock = new String("")
  void method() {
    synchronized(lock) {
      //业务代码
    }
   } 

  void method() {
    String lock = new String("")
    synchronized(lock) {
      //业务代码
    }
   } 
}
```
要注意调用时，是否时同一个lock对象，要用同一个lock对象，否则没有同步作用了。（例如service类的多个实例之间用this，调用method时，lock对象在方法内新建的。这两种情况都会失去同步作用）
##3.lock
###ReentrantLock
1.LinkedBlockingQueue中ReentrantLock搭配condition使用
>https://www.jianshu.com/p/3061933c7be1
#三、参与过项目中的使用场景
#四、源码中的注释
源码中有非常多的注释，对于理解原理并使用很有帮助
##1.ReentrantReadWriteLock
可重入读写锁

An implementation of ReadWriteLock supporting similar semantics to ReentrantLock.
ReadWriteLock的一种实现，支持与ReentrantLock类似的语义。

This class has the following properties:
此类具有以下属性：

**Acquisition order**
获取顺序

This class does not impose a reader or writer preference ordering for lock access. However, it does support an optional fairness policy.
此类不会对锁访问强制执行读写偏好排序。然而，它确实支持可选的公平政策。

**Non-fair mode (default)**
非公平模式（默认）

When constructed as non-fair (the default), the order of entry to the read and write lock is unspecified, subject to reentrancy constraints. A nonfair lock that is continuously contended may indefinitely postpone one or more reader or writer threads, but will normally have higher throughput than a fair lock.
当构造为非公平（默认）时，读写锁的输入顺序未指定，受可重入性约束的约束。持续争用的非空锁可能会无限期地延迟一个或多个读写器线程，但通常比公平锁具有更高的吞吐量。

**Fair mode**
公平模式

When constructed as fair, threads contend for entry using an approximately arrival-order policy. When the currently held lock is released, either the longest-waiting single writer thread will be assigned the write lock, or if there is a group of reader threads waiting longer than all waiting writer threads, that group will be assigned the read lock.
当构造为公平时，线程使用近似到达顺序策略来竞争条目。释放当前持有的锁时，将为等待时间最长的单个写入线程分配写锁，或者如果有一组读线程比所有等待的写入线程等待时间更长，则将为该组分配读锁。

A thread that tries to acquire a fair read lock (non-reentrantly) will block if either the write lock is held, or there is a waiting writer thread. 
尝试获取公平读取锁（非可重入）的线程将在写入锁被持有或存在等待写入线程时阻塞。

The thread will not acquire the read lock until after the oldest currently waiting writer thread has acquired and released the write lock.
在当前等待的最旧写入线程获取并释放写锁之前，该线程不会获取读锁。

 Of course, if a waiting writer abandons its wait, leaving one or more reader threads as the longest waiters in the queue with the write lock free, then those readers will be assigned the read lock.
当然，如果等待的写入程序放弃了等待，将一个或多个读线程作为队列中最长的等待程序，而写锁处于空闲状态，那么这些读卡器将被分配读锁。

A thread that tries to acquire a fair write lock (non-reentrantly) will block unless both the read lock and write lock are free (which implies there are no waiting threads).
试图获取公平写锁（非可重入）的线程将阻塞，除非读锁和写锁都是空闲的（这意味着没有等待的线程）。

 (Note that the non-blocking ReentrantReadWriteLock.ReadLock.tryLock() and ReentrantReadWriteLock.WriteLock.tryLock() methods do not honor this fair setting and will immediately acquire the lock if it is possible, regardless of waiting threads.)
（请注意，非阻塞的ReentrantReadWriteLock.ReadLock.tryLock（）和ReentrantReadWriteLock。WriteLock。tryLock（）方法不遵守此公平设置，如果可能，将立即获取锁，而不管等待的线程如何。）

**Reentrancy**
可重入

This lock allows both readers and writers to reacquire read or write locks in the style of a ReentrantLock. Non-reentrant readers are not allowed until all write locks held by the writing thread have been released.
该锁允许读写器以可重入锁的方式重新获取读或写锁。在释放写入线程持有的所有写入锁之前，不允许使用非可重入读取。

Additionally, a writer can acquire the read lock, but not vice-versa. Among other applications, reentrancy can be useful when write locks are held during calls or callbacks to methods that perform reads under read locks. If a reader tries to acquire the write lock it will never succeed.
此外，写入程序可以获取读锁，但反之亦然。在其他应用程序中，当在调用或回调在读锁下执行读取的方法时持有写锁时，可重入性非常有用。如果读取试图获取写入锁，它将永远不会成功。

**Lock downgrading**
锁降级

Reentrancy also allows downgrading from the write lock to a read lock, by acquiring the write lock, then the read lock and then releasing the write lock. However, upgrading from a read lock to the write lock is not possible.
可重入性还允许从写锁降级为读锁，方法是先获取写锁，然后获取读锁，然后释放写锁。但是，不可能从读锁升级到写锁。

Interruption of lock acquisition
锁获取中断

The read lock and write lock both support interruption during lock acquisition.
读锁和写锁都支持在锁获取期间中断。

**Condition support**
条件支持

The write lock provides a Condition implementation that behaves in the same way, with respect to the write lock, as the Condition implementation provided by ReentrantLock.newCondition does for ReentrantLock. This Condition can, of course, only be used with the write lock.
写锁提供的条件实现与ReentrantLock提供的条件实现在写锁方面的行为方式相同。新条件不适用于ReentrantLock。当然，这种情况只能与写锁一起使用。

The read lock does not support a Condition and readLock().newCondition() throws UnsupportedOperationException.
读锁不支持条件和读锁（）。newCondition（）抛出UnsupportedOperationException。

**Instrumentation**
状态监测
This class supports methods to determine whether locks are held or contended. These methods are designed for monitoring system state, not for synchronization control.
Serialization of this class behaves in the same way as built-in locks: a deserialized lock is in the unlocked state, regardless of its state when serialized.
这个类支持确定锁是被持有还是被争用的方法。这些方法是为监控系统状态而设计的，而不是为同步控制而设计的。

此类的序列化与内置锁的行为相同：反序列化的锁处于解锁状态，而不管序列化时的状态如何。

Sample usages. Here is a code sketch showing how to perform lock downgrading after updating a cache (exception handling is particularly tricky when handling multiple locks in a non-nested fashion):
  示例用法。下面是一个代码草图，展示了如何在更新缓存后执行锁降级（当以非嵌套方式处理多个锁时，异常处理尤其棘手）：
```
 class CachedData {
   Object data;
   volatile boolean cacheValid;
   final ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();

   void processCachedData() {
     rwl.readLock().lock();
     if (!cacheValid) {
       // Must release read lock before acquiring write lock
//在获取写锁之前必须释放读锁
       rwl.readLock().unlock();
       rwl.writeLock().lock();
       try {
         // Recheck state because another thread might have
         // acquired write lock and changed state before we did.
//重新检查状态，因为另一个线程可能在我们之前获得了写锁并更改了状态。
         if (!cacheValid) {
           data = ...
           cacheValid = true;
         }
         // Downgrade by acquiring read lock before releasing write lock
//通过在释放写锁之前获取读锁来降级
         rwl.readLock().lock();
       } finally {
         rwl.writeLock().unlock(); // Unlock write, still hold read
//释放写入锁，保持读取锁
       }
     }

     try {
       use(data);
     } finally {
       rwl.readLock().unlock();
     }
   }
 }
```
ReentrantReadWriteLocks can be used to improve concurrency in some uses of some kinds of Collections. This is typically worthwhile only when the collections are expected to be large, accessed by more reader threads than writer threads, and entail operations with overhead that outweighs synchronization overhead. For example, here is a class using a TreeMap that is expected to be large and concurrently accessed.
 ReentrantReadWriteLocks可用于在某些集合的某些用途中提高并发性。只有当集合预计很大，读线程比写线程多，并且需要的操作开销超过同步开销时，这通常才有价值。例如，下面是一个使用树映射的类，该树映射应该是大型的，并且可以并发访问。
```
 class RWDictionary {
   private final Map<String, Data> m = new TreeMap<>();
   private final ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
   private final Lock r = rwl.readLock();
   private final Lock w = rwl.writeLock();

   public Data get(String key) {
     r.lock();
     try { return m.get(key); }
     finally { r.unlock(); }
   }
   public List<String> allKeys() {
     r.lock();
     try { return new ArrayList<>(m.keySet()); }
     finally { r.unlock(); }
   }
   public Data put(String key, Data value) {
     w.lock();
     try { return m.put(key, value); }
     finally { w.unlock(); }
   }
   public void clear() {
     w.lock();
     try { m.clear(); }
     finally { w.unlock(); }
   }
 }
```
**Implementation Notes**
This lock supports a maximum of 65535 recursive write locks and 65535 read locks. Attempts to exceed these limits result in Error throws from locking methods.
实施说明
此锁最多支持65535个递归写锁和65535个读锁。试图超过这些限制会导致锁定方法抛出错误。
Since:
1.5
Author:
Doug Lea





##2.ReadWriteLock
读写锁

A ReadWriteLock maintains a pair of associated locks, one for read-only operations and one for writing. The read lock may be held simultaneously by multiple reader threads, so long as there are no writers. The write lock is exclusive.
ReadWriteLock维护一对关联锁，一个用于只读操作，一个用于写入。只要没有写入程序，读锁可以由多个读线程同时保持。写锁是独占的。

All ReadWriteLock implementations must guarantee that the memory synchronization effects of writeLock operations (as specified in the Lock interface) also hold with respect to the associated readLock. That is, a thread successfully acquiring the read lock will see all updates made upon previous release of the write lock.
所有ReadWriteLock实现都必须保证writeLock操作（如Lock接口中所指定）的内存同步效果也适用于相关的readLock。也就是说，成功获取读锁的线程将看到在上一次发布写锁时进行的所有更新。

A read-write lock allows for a greater level of concurrency in accessing shared data than that permitted by a mutual exclusion lock. 
读写锁允许在访问共享数据时比互斥锁允许的并发级别更高。

It exploits the fact that while only a single thread at a time (a writer thread) can modify the shared data, in many cases any number of threads can concurrently read the data (hence reader threads). 
它利用了这样一个事实：虽然一次只有一个线程（写入线程）可以修改共享数据，但在许多情况下，任何数量的线程都可以同时读取数据（因此读取线程）。

In theory, the increase in concurrency permitted by the use of a read-write lock will lead to performance improvements over the use of a mutual exclusion lock. In practice this increase in concurrency will only be fully realized on a multi-processor, and then only if the access patterns for the shared data are suitable.
理论上，与互斥锁相比，使用读写锁所允许的并发性增加将导致性能提高。在实践中，这种并发性的增加只有在多处理器上才能完全实现，并且只有在共享数据的访问模式合适的情况下才能实现。

Whether or not a read-write lock will improve performance over the use of a mutual exclusion lock depends on the frequency that the data is read compared to being modified, the duration of the read and write operations, and the contention for the data - that is, the number of threads that will try to read or write the data at the same time. 
读写锁是否会比互斥锁的使用提高性能，取决于读取数据的频率（与被修改的频率相比）、读写操作的持续时间以及对数据的争用——也就是说，同时尝试读取或写入数据的线程数。

For example, a collection that is initially populated with data and thereafter infrequently modified, while being frequently searched (such as a directory of some kind) is an ideal candidate for the use of a read-write lock. 
例如，一个集合最初填充了数据，之后很少被修改，同时经常被搜索（例如某种目录），这是使用读写锁的理想候选。

However, if updates become frequent then the data spends most of its time being exclusively locked and there is little, if any increase in concurrency. 
然而，如果更新变得频繁，那么数据的大部分时间都被独占锁定，并发性几乎没有增加。

Further, if the read operations are too short the overhead of the read-write lock implementation (which is inherently more complex than a mutual exclusion lock) can dominate the execution cost, particularly as many read-write lock implementations still serialize all threads through a small section of code.
此外，如果读操作太短，读写锁实现的开销（其本质上比互斥锁更复杂）可能会主导执行成本，尤其是当许多读写锁实现仍然通过一小段代码序列化所有线程时。

 Ultimately, only profiling and measurement will establish whether the use of a read-write lock is suitable for your application.
最终，只有分析和测量才能确定读写锁的使用是否适合您的应用程序。

Although the basic operation of a read-write lock is straight-forward, there are many policy decisions that an implementation must make, which may affect the effectiveness of the read-write lock in a given application. Examples of these policies include:
尽管读写锁的基本操作是直接的，但实现必须做出许多策略决策，这可能会影响特定应用程序中读写锁的有效性。这些政策的例子包括：

Determining whether to grant the read lock or the write lock, when both readers and writers are waiting, at the time that a writer releases the write lock. 
在写入程序释放写锁时，当读取线程和写入线程都在等待时，确定是授予读锁还是写入锁。

Writer preference is common, as writes are expected to be short and infrequent. 
写入优先是常见的，因为写入操作时间短且不频繁。

Reader preference is less common as it can lead to lengthy delays for a write if the readers are frequent and long-lived as expected. 
读取优先不太常见，因为如果读取操作像预期的那样频繁且长寿，那么它可能会导致写入线程的长时间延迟。

Fair, or "in-order" implementations are also possible.
公平或“有序”实现也是可能的。

Determining whether readers that request the read lock while a reader is active and a writer is waiting, are granted the read lock.
确定在读取线程处于活动状态且写入线程正在等待时请求读锁的读取线程是否被授予读锁。

 Preference to the reader can delay the writer indefinitely, while preference to the writer can reduce the potential for concurrency.
读取优先会无限期地延迟写入线程，而写入优先可以减少并发的可能性。

Determining whether the locks are reentrant: can a thread with the write lock reacquire it? Can it acquire a read lock while holding the write lock? Is the read lock itself reentrant?
确定锁是否可重入：具有写锁的线程能否重新获取它？它能在保持写锁的同时获得读锁吗？读锁本身是可重入的吗？


Can the write lock be downgraded to a read lock without allowing an intervening writer? Can a read lock be upgraded to a write lock, in preference to other waiting readers or writers?
写锁是否可以降级为读锁，而不允许插入写入线程？读锁是否可以升级为写锁，优先于其他等待的读取线程或写入线程？

You should consider all of these things when evaluating the suitability of a given implementation for your application.
在评估应用程序的给定实现的适用性时，应考虑所有这些事项。

Since:
1.5
See Also:
ReentrantReadWriteLock, Lock, ReentrantLock
Author:
Doug Lea

##3.ReentrantLock
可重入锁

A reentrant mutual exclusion Lock with the same basic behavior and semantics as the implicit monitor lock accessed using synchronized methods and statements, but with extended capabilities.
一个可重入互斥锁，其基本行为和语义与使用同步方法和语句访问的隐式监视锁相同，但具有扩展功能。

A ReentrantLock is owned by the thread last successfully locking, but not yet unlocking it. 
ReentrantLock属于上次成功锁定但尚未解锁的线程。

A thread invoking lock will return, successfully acquiring the lock, when the lock is not owned by another thread. 
当锁不属于另一个线程时，调用锁的线程将返回并成功获取锁。

The method will return immediately if the current thread already owns the lock. 
如果当前线程已经拥有锁，该方法将立即返回。

This can be checked using methods isHeldByCurrentThread, and getHoldCount.
这可以使用isHeldByCurrentThread和getHoldCount方法进行检查。

The constructor for this class accepts an optional fairness parameter. 
此类的构造函数接受可选的公平性参数。

When set true, under contention, locks favor granting access to the longest-waiting thread.
当设置为true时，在争用状态下，锁有利于向等待时间最长的线程授予访问权限。

 Otherwise this lock does not guarantee any particular access order. 
否则，该锁不保证任何特定的访问顺序。

Programs using fair locks accessed by many threads may display lower overall throughput (i.e., are slower; often much slower) than those using the default setting, but have smaller variances in times to obtain locks and guarantee lack of starvation. 
与使用默认设置的程序相比，使用由多个线程访问的公平锁的程序可能会显示较低的总体吞吐量（即较慢；通常要慢得多），但在获得锁和保证没有饥饿的时间上差异较小。

Note however, that fairness of locks does not guarantee fairness of thread scheduling.
但是请注意，锁的公平性并不能保证线程调度的公平性。

 Thus, one of many threads using a fair lock may obtain it multiple times in succession while other active threads are not progressing and not currently holding the lock. 
因此，使用公平锁的多个线程中的一个可能会连续多次获得公平锁，而其他活动线程则不会继续进行，当前也不会持有该锁。

Also note that the untimed tryLock() method does not honor the fairness setting. 
还要注意，untimed tryLock（）方法不支持公平性设置。

It will succeed if the lock is available even if other threads are waiting.
如果锁可用，即使其他线程正在等待，它也会成功。

It is recommended practice to always immediately follow a call to lock with a try block, most typically in a before/after construction such as:
建议在调用后立即使用try 代码块，最典型的是在构造前/构造后，例如：

  ```
 class X {
   private final ReentrantLock lock = new ReentrantLock();
   // ...

   public void m() {
     lock.lock();  // block until condition holds
     try {
       // ... method body
     } finally {
       lock.unlock()
     }
   }
 }
```
In addition to implementing the Lock interface, this class defines a number of public and protected methods for inspecting the state of the lock. 
除了实现锁接口之外，这个类还定义了许多公共和受保护的方法来检查锁的状态。

Some of these methods are only useful for instrumentation and monitoring.
其中一些方法仅适用于显示状态和监测。

Serialization of this class behaves in the same way as built-in locks: a deserialized lock is in the unlocked state, regardless of its state when serialized.
此类的序列化与内置锁的行为相同：反序列化的锁处于解锁状态，而不管序列化时的状态如何。

This lock supports a maximum of 2147483647 recursive locks by the same thread. Attempts to exceed this limit result in Error throws from locking methods.
此锁最多支持同一线程的2147483647个递归锁。试图超过此限制会导致锁定方法抛出错误。

Since:
1.5
Author:
Doug Lea
#五、原理分析
（先跳过吧，网上一搜一大堆,其实不太推荐在学习怎么使用的阶段去看这些，**学习如何使用，看源码里的注释就好了，很详细，参考第四节**）
