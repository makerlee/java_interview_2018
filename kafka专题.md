下列问题的回答都基于0.9.x及以上版本
所有问题都可以在[这儿](http://www.importnew.com/25247.html)找到答案
### kafka节点之间如何复制备份的？
1. ISR In-Sync Replicas
2. HW HighWatermark
3. LEO Log End Offset
### Kafka消息是否会丢失？为什么？
1. 当然会丢失
2. at most once 当producer的retry设置为0
### kafka最合理的配置？
- 没有最合理，根据业务场景不同在可靠性和性能之间权衡
- 当然要告诉面试官至少一种配置 exactly once
### kafka leader的选举机制？
- Kafka在Zookeeper中为每一个partition动态的维护了一个ISR，这个ISR里的所有replica都跟上了leader，只有ISR里的成员才能有被选为leader的可能
- 在ISR中至少有一个follower时，Kafka可以确保已经commit的数据不丢失，但如果某一个partition的所有replica都挂了，就无法保证数据不丢失了。这种情况下有两种可行的方案：
  1. 等待ISR中任意一个replica“活”过来，并且选它作为leader
  2. 选择第一个“活”过来的replica（并不一定是在ISR中）作为leader(默认)
### kafka的消息保证有几种方式
1. at most once 至少一次
2. at least once 至多一次
3. exactly once 正好一次 需要编码和配置来保证