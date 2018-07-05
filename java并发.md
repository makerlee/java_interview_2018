### synchronized 的实现原理以及锁优化？
1. 参考:http://blog.csdn.net/javazejian/article/details/72828483
2. 优化要多扯,大谈特谈。具体参考《java并发编程实战》,讲的比较系统
### synchronized 在静态方法和普通方法的区别？
1. synchronized修饰静态方法，实际上是对该类对象(.class)加锁 俗称“类锁”
2. 修饰非静态方法，是对调用该方法的对象实例加锁，俗称“对象锁”
3. 一个对象在两个线程中分别调用一个静态synchronized方法和一个非静态
synchronized方法 不会产生互斥
### synchronized 和 lock 有什么区别？
- 参考：http://blog.csdn.net/natian306/article/details/18504111
- ReentrantLock在加锁和内存上的语义和内置相同，此外它还提供了锁等待、可中断、公平性这几个特有功能
1. 用法区别：
    1. sychronized 可以用在方法上；用在代码块，括号中指定锁对象
    2. 需要显示指定起始位置和终止位置。一般使用ReentrantLock类做为锁，
      多个线程中必须要使用一个ReentrantLock类做为对象才能保证锁的生效。
      且在加锁和解锁处需要通过lock()和unlock()显示指出。所以一般会在finally块中写unlock()以防死锁。
2. 性能区别：
    1. jdk1.6对sychronized做了很大的优化
3. 用途区别：
    1. lock能实现一些个性化的需求
### 分段锁的原理,锁粒度减小的思考
1. 总体思想就是减小锁的粒度,
2. 减少锁的竞争
3. 结合concurrentHashMap来说，要不然不好表达
### volatile 的实现原理？
- JSR-133 JAVA内存模型与线程规范 对volatile的语义进行了重新规范
- http://www.cnblogs.com/dolphin0520/p/3920373.html
- 下面这段话摘自《深入理解Java虚拟机》：
“观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令”
- lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：

　　	1）它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；
        即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
        
　　	2）它会强制将对缓存的修改操作立即写入主存；

　　	3）如果是写操作，它会导致其他CPU中对应的缓存行无效。
### CAS 有什么缺陷，如何解决？
- 参考：http://zl198751.iteye.com/blog/1848575
- CAS通过调用JNI的代码实现的。JNI:Java Native Interface为JAVA本地调用，允许java调用其他语言。
而compareAndSwapInt就是借助C来调用CPU底层指令实现的。
1. ABA问题，通过给数据加版本可以现实区分
2. 自旋等待时间过长，造成CPU资源浪费

### AQS
1. 全称AbstractQueuedSynchronizer 是JUC下提供的一套用于实现基于FIFO等待队列的阻塞锁和相关的同步器的一个同步框架
2. CountdownLatch、ReentrantLock、Semaphore、FutureTask(新版本不是依赖AQS了)都是基于AQS来实现的
### 如何检测死锁？怎么预防死锁？
- https://juejin.im/post/5aaf6ee76fb9a028d3753534
1. 死锁分类：
    1. 锁顺序死锁
    2. 动态锁顺序死锁
    3. 在协作对象之间的死锁
    4. 资源死锁： 线程池死锁、网络连接池死锁
2. 死锁检测工具：
    1. Jstack ---> jstack -F pid 
    2. JConsole ---> jconsole 会出来图形界面
3. 死锁的预防：
    1. 以确定的顺序获取锁
    2. Lock接口提供的tryLock(long time) 锁等待超时 自动放弃
### Java 内存模型？
- JMM是围绕着在并发过程中如何处理*原子性*、*可见性*和*有序性*这3个特征来建立的.
1. 原子性：JMM直接保证原子性变量操作 load read assign use write store
2. 可见性：当一个线程修改了共享变量的值，其他线程可以立刻得知这个修改。volatile、synchronized、final
3. 有序性：单线程总是有序的；多线程靠volatile和synchronized保证线程之间操作的有序性
4. happen-before 先行发生原则
### 如何保证多线程下 i++ 结果正确？
1. sychronized同步方法或者代码块
2. 原子类
### 线程池的种类，区别和使用场景？
- 参考:http://gityuan.com/2016/01/16/thread-pool/
1. JDK针对不同需求，利用Executors类提供了4种不同的线程池(还有一些组合的)：
	  newCachedThreadPool, newFixedThreadPool, newScheduledThreadPool, newSingleThreadExecutor
2. 
### 分析线程池的实现原理和线程的调度过程？
- 见上题链接
### 线程池如何调优，最大数目如何确认？
1. 需要针对具体情况而具体处理，不同的任务类别应采用不同规模的线程池，
任务类别可划分为： “CPU密集型任务”、“IO密集型任务“ 和 “混合型任务”

2. 利用线程池提供的参数进行监控

3. 通过扩展线程池进行监控,见上题链接
### ThreadLocal原理，用的时候需要注意什么？
1. https://blog.csdn.net/lovesomnus/article/details/64441426
2. https://droidyue.com/blog/2016/03/13/learning-threadlocal-in-java/
### CountDownLatch 和 CyclicBarrier 、Semaphore的用法，以及相互之间的差别?
- https://www.cnblogs.com/dolphin0520/p/3920397.html
- 他们都是java并发编程里的辅助类，位于java.util.concurrent
1. CountDownLatch 正如其名，类似于一个计数器。
2. CyclicBarrier字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行
3. 信号量 与前两个区别很大
### 怎么实现所有线程在等待某个事件的发生才会去执行？
- CyclicBarrier实现
### LockSupport工具
1. LockSupport是用来创建锁和其他同步类的基本线程阻塞原语。
2. 主要方法：park()、 unpark(Thread t)
### Condition接口及其实现原理
1.
### Fork/Join框架的理解
- 参考：http://www.infoq.com/cn/articles/fork-join-introduction
### 八种阻塞队列以及各个阻塞队列的特性
1. ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。
2. LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。
3. PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。
4. DelayQueue：一个使用优先级队列实现的无界阻塞队列。
5. SynchronousQueue：一个不存储元素的阻塞队列。
6. LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。
7. LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。
