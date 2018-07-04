### 面向对象基本特征
- 什么是对象？对象有划分的边界，面试官其实想问 你怎么证明你能划分出对象，以及是合理的
- 封装
- 继承
- 多态
 ![avatar](http://www.cnitblog.com/images/cnitblog_com/lily/1972/o_OOBase.gif)

### final, finally, finalize 的区别
- final关键字:http://www.importnew.com/7553.html
- finally 用于try语句块
- finalize()是Object类的一个方法,与GC有关
### int 和 Integer 有什么区别
- int是基本数据类型 初始化值0
- Integer是int的封装类 初始化值null
- 自动装箱 自动拆箱的细节
### 重载和重写的区别
- override 重写是**父子类之间**多态性的一种体现
- overload 重载是**同一个类中**多态性的一种体现
### 抽象类和接口有什么区别
- 这题太难了 拒绝回答
### 说说反射的用途及实现
- 用途的话 最常见的加载数据库驱动
- 最重要的用途就是开发各种框架 比如spring的IOC、AOP都有用到
### 说说自定义注解的场景及实现
- Annotation型定义为@interface
- 举个例子 现在有个需求：通过aop监控接口调用的总时间(请求到来-返回响应)，
但是只要监控部分接口，有的需要监控。这时可以自定义一个注解，在需要监控的
接口函数上加上此注解，配合AOP即可实现。Student.getClass.getAnnotation(NeedMonitor.class)
### HTTP 请求的 GET 与 POST 方式的区别
- https://www.zhihu.com/question/28586791
- https://www.oschina.net/news/77354/http-get-post-different
- 按照上边这个博客的说法, 他们的没什么区别
### session 与 cookie 区别
- 参考:https://www.zhihu.com/question/19786827
- 为什么要有session机制
- 由于http协议是无状态的 session用来跟踪用户状态
### session 分布式处理
- session复制
- session粘滞
- session集中管理
### equals 与 == 的区别
- 还有公司会问？
- 一定记得要分 基本*数据类型*和*普通对象*
### wait和sleep的区别
1.
### 数组在内存中如何分配
1. 数组也是对象 JVM和普通java对象一样处理数组
2. 没有真正的多维数组
### 异常分类以及处理机制
        	throwable
        		Error                 jvm处理
        		Exception  
        			checked Exception 需要在方法上声明 向上抛出
        			runtime Exception 不必需处理
### Cloneable接口实现原理     	
1. 对象要想重写Object的clone()方法，必须要实现Cloneable
2. 设计不合理，apache common更方便
### wait和sleep的区别
- https://www.zhihu.com/question/23328075
1. 首先，要记住这个差别，“sleep是Thread类的方法,wait是Object类的方法”。尽管这两个方法都会影响线程的执行行为，但是本质上是有区别的。
2. Thread.sleep不会导致锁行为的改变，如果当前线程是拥有锁的，那么Thread.sleep不会让线程释放锁
3. 调用wait后，需要别的线程执行notify/notifyAll才能够重新获得CPU执行时间(唤醒线程)
