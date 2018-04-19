下列问题的回答都基于0.9.x及以上版本
所有问题都可以在[这儿](http://www.importnew.com/25247.html)找到答案
### kafka节点之间如何复制备份的？
- Kafka每个topic的partition有N个副本(replicas), N个replicas中，其中一个replica为leader，其他都为follower, leader处理partition的所有读写请求，与此同时，follower会被动定期地去复制leader上的数据
- 如果leader发生故障或挂掉，一个新leader被选举并被接受客户端的消息成功写入。Kafka确保从ISR中选举一个副本为leader
1. ISR = In-Sync Replicas [AR = ISR + OSR]
2. HW = HighWatermark
3. LEO = Log End Offset
### Kafka消息是否会丢失？为什么？
1. 当producer向leader发送数据时，可以通过request.required.acks参数来设置数据可靠性的级别
    1. 1 默认 leader已成功收到数据并得到确认后发送下一条message
    2. 0 producer无需等待来自broker的确认而继续发送下一批消息 可靠性最低
    3. -1 producer需要等待ISR中的所有follower都确认接收到数据后才算一次发送完成，
    可靠性最高。但是这样也不能保证数据不丢失，比如当ISR中只有leader时
2. 如果要提高数据的可靠性，在设置request.required.acks=-1的同时，也要min.insync.replicas这个参数
### kafka最合理的配置？
- 没有最合理，根据业务场景不同在可靠性和性能之间权衡
- 当然要告诉面试官至少一种配置 exactly once
### kafka leader的选举机制？
- Kafka在Zookeeper中为每一个partition动态的维护了一个ISR，这个ISR里的所有replica都跟上了leader，只有ISR里的成员才能有被选为leader的可能
- 在ISR中至少有一个follower时，Kafka可以确保已经commit的数据不丢失，但如果某一个partition的所有replica都挂了，就无法保证数据不丢失了。这种情况下有两种可行的方案：
  1. 等待ISR中任意一个replica“活”过来，并且选它作为leader
  2. 选择第一个“活”过来的replica（并不一定是在ISR中）作为leader(默认)
### kafka的消息保证有几种方式
- 0.11.x之后从语义上提供了exactly once, 之前的版本需要从业务层面实现
1. at most once 至少一次
2. at least once 至多一次
3. exactly once 正好一次 需要编码和配置来保证
### 高可靠配置
1. topic的配置：replication.factor>=3,即副本数至少是3个；2<=min.insync.replicas<=replication.factor
2. broker的配置：leader的选举条件unclean.leader.election.enable=false
3. producer的配置：request.required.acks=-1(all)，producer.type=sync