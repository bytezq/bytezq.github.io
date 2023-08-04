[kafka日志删除](https://blog.csdn.net/michael_f2008/article/details/116596301)

说明：
所用的Kafka版本为：kafka_2.12-1.1.0；
本文中所谈及的“日志”是指Kafka的数据缓存；
本文中所有的配置项，均位于kafka_2.12-1.1.0/config/server.properties文件中；
为描述方便，假设：log.dirs=/data/kafka-logs，即日志文件保存在/data/kafka-logs路径下；有一个topic名称为test001，partition为3。
1.Kafka日志留存类型
目前，Kafka的日志留存类型有2种：

delete
compact
本文只涉及delete机制。

2. Kafka日志留存机制
目前，Kafka的日志留存机制一共有3种：

基于空间维度
基于时间维度
基于起始位移维度
本文只涉及前2种机制。

3.基于文件时间的日志留存策略
可配置的参数如下：

数据留存时间（7天）：log.retention.hours=168
检查间隔时间（5分钟）：log.retention.check.interval.ms=300000
每个日志文件大小（1GB）：log.segment.bytes=1073741824
执行实际删除操作的间隔时间（15秒）：log.cleaner.backoff.ms=15000
以上的配置，是针对所有topic的每一个partition，即全局设置。也可以针对个别的topic进行单独设置，本文不作介绍。以上配置的作用如下：

在/data/kafka-logs/test001-0路径下，会有多个后缀为.log的日志文件，且大小都为1GB（除最新的日志文件外）

有一个程序，每5分钟会检查一次是否有日志文件的“最大时间戳”与当前时间的差值超过了7天，如果有，则将这个日志文件的后缀加上.delete

还有一个程序，每15秒会检查一次是否有标记为.delete的日志文件，如果有，则执行删除操作

4.基于文件大小的日志留存策略
可配置的参数如下：

文件所占空间的阈值（2GB）：log.retention.bytes=2147483648
检查间隔时间（5分钟）：log.retention.check.interval.ms=300000
每个日志文件大小（1GB）：log.segment.bytes=1073741824
执行实际删除操作的间隔时间（15秒）：log.cleaner.backoff.ms=15000
以上的配置，是针对所有topic的每一个partition，即全局设置。也可以针对个别的topic进行单独设置，本文不作介绍。以上配置的作用如下：

在/data/kafka-logs/test001-0路径下，会有多个后缀为.log的日志文件，且大小都为1GB（除最新的日志文件外）

有一个程序，每5分钟会统计一次当前路径下所有日志文件大小，如果总大小超过阈值（2GB）的部分多于一个日志文件的大小（1GB），则将最早的日志文件的后缀加上.delete

还有一个程序，每15秒会检查一次是否有标记为.delete的日志文件，如果有，则执行删除操作

举个例子：
   日志文件大小为1GB，当前路径下(/data/kafka-logs/test001-0)有3个日志文件，分别为1GB、1GB、100MB，显然100MB的日志文件为系统正在使用的。此时，若第一个程序开始统计，则总大小为2GB+100MB，由于阈值设为2GB，则超过阈值的部分为100MB，而100MB小于1GB（单个日志文件的大小），所以此时不会有任何文件被标记为.delete，也不会有任何文件被删除。
   以此类推，在第3个日志文件达到1GB之前，都是这种情况。
   当第3个日志文件达到1GB，并开始写第4个日志文件时（假设当前大小为200KB），若第一个程序开始统计，则总大小为3GB+200KB，超过阈值的部分为1GB+100KB，已经大于了1GB，所以最早的日志文件会被标记为.delete，再过15秒之后，第二个程序会将这个文件删除。

5.总结
基于文件时间和基于文件大小的两种留存策略，需要分别设置log.retention.hours和log.retention.bytes两个参数。
设置了哪个参数，则哪种策略生效，如果同时都设置了，则哪个条件先满足，哪个策略生效。

需要注意的是， 以上的配置对每个topic的每个partition都有效，也就是说，如果一个topic有多个partition，则每个partition的日志文件路径下都执行上述策略。因此，在实际配置时，一定要把partition的数量考虑进去！

参考资料
Kafka学习笔记之Kafka日志删出策略
kafka log.retention.bytes
Kafka主要参数详解
what is the difference between log.cleaner.delete.retention.ms and log.retention.ms in Kafka