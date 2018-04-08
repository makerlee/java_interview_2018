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
1. 总体思想就是减小锁的粒度
2. 结合concurrentHashMap来说，要不然不好表达
### volatile 的实现原理？
- http://www.cnblogs.com/dolphin0520/p/3920373.html
- 下面这段话摘自《深入理解Java虚拟机》：
　　	“观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，
	加入volatile关键字时，会多出一个lock前缀指令”

- lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：

　　	1）它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；
        即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
        
　　	2）它会强制将对缓存的修改操作立即写入主存；

　　	3）如果是写操作，它会导致其他CPU中对应的缓存行无效。
### CAS 有什么缺陷，如何解决？
- 目前我只知道ABA问题，通过给数据加版本可以现实区分
### Hashtable 是怎么加锁的 ？
1. 对数据进行读写的方法都加上synchronized的修饰
### ConcurrentHashMap 介绍？1.8 中为什么要用红黑树？
1.
2.
### AQS
1.
2.
### 如何检测死锁？怎么预防死锁？
1.
2.
3.
### Java 内存模型？
1.
2.
3.
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
1.
2.
### CountDownLatch 和 CyclicBarrier 、Semaphore的用法，以及相互之间的差别?
- https://www.cnblogs.com/dolphin0520/p/3920397.html
- 他们都是java并发编程里的辅助类，位于java.util.concurrent
1. CountDownLatch 正如其名，类似于一个计数器。
2. CyclicBarrier字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行
3. 信号量 与前两个区别很大
### 怎么实现所有线程在等待某个事件的发生才会去执行？
- CyclicBarrier实现
### LockSupport工具
1.
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
