集合，其实就是HashMap和ConcurrentHashMap.前者主要考数据结构和扩容；后者重点在分段锁。
<hr>

### List 和 Set 的区别
1. List可以元素重复，Set不允许有重复元素(不存在e1.equals(e2))
2. list是有序的 set是无序的(treeSet是有序的)
### HashSet 是如何保证不重复的
1. 底层是hashMap的实现
### HashMap 是线程安全的吗，为什么不是线程安全的
1. 不是线程安全类
### HashMap 的扩容过程
1. 参考:https://www.jianshu.com/p/f6730d5784ad
2. 回答这题一定要说明是基于JDK1.7还是1.8，因为实现完全不一样
### HashMap 1.7 与 1.8的区别，说明1.8做了哪些优化
1. 扩容的区别，1.8加入了红黑树的数据结构
2.
### 强引用 、软引用、 弱引用、虚引用
1. 强引用 Student a = new Student()
2.
3.
4.
### Hashtable 是怎么加锁的 ？
1. 对数据进行读写的方法都加上synchronized的修饰
### ConcurrentHashMap 介绍？1.8 中为什么要用红黑树？
1. 
2. 
### Arrays.sort 实现原理
1. 策略设计模式(strategy pattern);
2. 归并排序(merge sort): 时间复杂度 n*log(n);
### LinkedHashMap的应用
1. 在网上查了 都说是可以基于LinkedHashMap实现LRU(least recently use)