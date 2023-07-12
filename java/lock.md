#JDK中的lock使用总结
##1.LinkedBlockingQueue中ReentrantLock搭配condition使用
LinkedBlockingQueue是基于单链表的阻塞队列
基于链接节点的可选有界阻塞队列。此队列对元素FIFO进行排序（先进先出）。队列的头是在队列上停留时间最长的元素。队列的尾部是在队列上停留时间最短的元素。新元素插入到队列的尾部，队列检索操作获取队列头部的元素。链接队列通常比基于阵列的队列具有更高的吞吐量，但在大多数并发应用程序中，其性能不太可预测。

可选的capacity-bound构造函数参数用于防止队列过度扩展。容量（如果未指定）等于整数。最大值。链接节点在每次插入时动态创建，除非这会使队列超过容量。

此类及其迭代器实现了集合和迭代器接口的所有可选方法。
![LinkedBlockingQueue提供的操作.png](https://upload-images.jianshu.io/upload_images/24860325-a95d02ddcdf2163a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先是核心的数据结构，LinkedBlockingQueue是基于单链表如下代码所示
```
static class Node<E> {
        E item;

        Node<E> next;

        Node(E x) { item = x; }
    }

    transient Node<E> head;

    private transient Node<E> last;
```
为了保证线程安全，声明一个读场景的锁，一个写场景的锁
```

    /** Lock held by take, poll, etc */
    private final ReentrantLock takeLock = new ReentrantLock();

    /** Wait queue for waiting takes */
    private final Condition notEmpty = takeLock.newCondition();

    /** Lock held by put, offer, etc */
    private final ReentrantLock putLock = new ReentrantLock();

    /** Wait queue for waiting puts */
    private final Condition notFull = putLock.newCondition();
```

###1.常用的加入队列方法
**无参的offer**
如果可以在不超过队列容量的情况下立即在此队列尾部插入指定的元素，则在成功时返回true，如果此队列已满，则返回false。使用容量受限队列时，此方法通常优于方法add，后者只能通过引发异常才能插入元素。

```
       public boolean offer(E e) {
        //非空判断
        if (e == null) throw new NullPointerException();
        final AtomicInteger count = this.count;
        //容量判断
        if (count.get() == capacity)
            return false;
        int c = -1;
        Node<E> node = new Node<E>(e);
        final ReentrantLock putLock = this.putLock;
        //当前线程，获取写入场景锁
        putLock.lock();
        try {
            if (count.get() < capacity) {
                //容量有剩余，加入队列尾部
                enqueue(node);
                c = count.getAndIncrement();
                if (c + 1 < capacity)
                    //加入之后，容量还是有剩余，唤醒写入线程，避免等待
                    //（如果没有余量，加入队列的操作就要等待）
                    notFull.signal();
            }
        } finally {
            putLock.unlock();
        }
        if (c == 0)
            signalNotEmpty();
        return c >= 0;
    }

```

**带参的offer，(实现等待指定时间，再加入队列)**
使用condition使写入线程进入等待
在该队列的尾部插入指定的元素，如有必要，等待指定的等待时间，等待容量变为有空余。
```
    public boolean offer(E e, long timeout, TimeUnit unit)
        throws InterruptedException {

        if (e == null) throw new NullPointerException();
        long nanos = unit.toNanos(timeout);
        int c = -1;
        final ReentrantLock putLock = this.putLock;
        final AtomicInteger count = this.count;
        //获取锁（如果线程被中断，就无法获取）
        putLock.lockInterruptibly();
        try {
            while (count.get() == capacity) {
                if (nanos <= 0L)
                    return false;
                //使写入线程进入等待
                nanos = notFull.awaitNanos(nanos);
            }
            enqueue(new Node<E>(e));
            c = count.getAndIncrement();
            if (c + 1 < capacity)
                notFull.signal();
        } finally {
            putLock.unlock();
        }
        if (c == 0)
            signalNotEmpty();
        return true;
    }
```
###2.poll（与offer使用方式一致）
```
    public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        E x = null;
        int c = -1;
        long nanos = unit.toNanos(timeout);
        final AtomicInteger count = this.count;
        final ReentrantLock takeLock = this.takeLock;
        takeLock.lockInterruptibly();
        try {
            while (count.get() == 0) {
                if (nanos <= 0L)
                    return null;
                nanos = notEmpty.awaitNanos(nanos);
            }
            x = dequeue();
            c = count.getAndDecrement();
            if (c > 1)
                notEmpty.signal();
        } finally {
            takeLock.unlock();
        }
        if (c == capacity)
            signalNotFull();
        return x;
    }
```
