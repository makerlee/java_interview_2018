### why netty
参考：https://blog.csdn.net/a953713428/article/details/65629552
1. 说java原生NIO的缺点
2. netty的优点：Netty是一个高性能、异步事件驱动的NIO框架，
   它提供了对TCP、UDP和文件传输的支持，作为一个异步NIO框架
### 常用tcp参数
- https://blog.csdn.net/yangguosb/article/details/79185799
1. SO_SNDBUF
2. SO_RCVBUF
3. TCP_NODELAY
4. SO_KEEPALIVE: 是否使用TCP的心跳机制
5. SO_REUSERADDR: 是否复用处于TIME_WAIT状态连接的端口
6. SO_BACKLOG
### 说说业务中，Netty 的使用场景
1. 分布式框架中rpc通信
2. 自定义应用层协议实现
### 原生的 NIO 在 JDK 1.7 版本存在 epoll bug
- https://blog.csdn.net/a925907195/article/details/73853771
1. cpu 100%
2. netty 屏蔽掉这个bug
### 什么是TCP 粘包/拆包,解决办法
1. tcp是基于stream的，他对上层应用层协议是无感知的。tcp segment的大小受MTU和网络状况
的影响可能会把一条应用消息分割成两次发送，也可能会把两条或多条应用消息组合发送
2. ①定长消息  ②分隔符  ③header+body header里用4个字节存储消息的长度  ④更复杂的应用层协议
### netty线程模型
- 参见《netty权威指南v2》P423
1. netty的线程模型不是一层不变的 通过设置不同的启动参数，netty可以同时支持reactor单线程模型
  、多线程模型、主从reactor多线程模型
2. NioEventLoopGroup：  bossGroup  workGroup 两个reactor线程池
      1. boss用于接受client的connection请求
      2. work用于处理I/O相关的读写操作
### 说说 Netty 的零拷贝
1. 在 OS 层面上的Zero-copy通常指避免在用户态(User-space)与内核态(Kernel-space)之间来回拷贝数据
2. Netty的 Zero-coyp 完全是在用户态(Java 层面)的, 它的 Zero-copy 的更多的是偏向于
   优化数据操作 这样的概念
    1. CompositeByteBuf
    2. ByteBuf  slice()
    3. ByteBuf  wrap()
    4. FileRegion java.nio FileChannel.transferTo()
### Netty 内部执行流程
1.
### Netty重连实现
- http://tterm.blogspot.com/2014/03/netty-tcp-client-with-reconnect-handling.html
1. channelFutureListener
2. inActive