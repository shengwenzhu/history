# 一、Kafka 定位

+ 消息中间件：具有系统解耦、冗余存储、流量削峰、缓冲、异步通信、扩展性、可恢复性、消息顺序性保障、回溯消费等功能；
+ 存储系统：将消息持久化到磁盘降低数据丢失的风险；
+ 流式处理平台：为每个流行的流式处理框架提供了可靠的数据来源，还提供了一个完整的流式处理类库，比如窗口、连接、变换和聚合等各类操作；

# 二、Kafka 体系架构

一个典型的 Kafka 体系架构包括若干 `Producer`、若干 `Broker`、若干 `Consumer`，以及一个 `ZooKeeper 集群`，如下图所示：

![kafka体系结构](image/kafka体系结构.PNG)

## 1. Producer（生产者）

Producer 即发送消息的一方，负责将消息发送到 Broker。









+ Consumer（消费者）：接收消息的一方，负责从 Broker 订阅并消费消息；

+ Broker（服务器代理节点）：一个独立的 Kafka 服务节点，负责将收到的消息存储到磁盘中；

+ ZooKeeper：对元数据信息进行管理，如集群、broker、主题、分区等。

+ 主题（Topic）

  **消息的类别**，生产者发送到 Kafka 集群的每一条消息都要指定一个主题，消费者通过订阅主题进而消费消息。

+ 分区（Partition）

  主题可以进一步细分为多个分区。

  从存储层面来看，分区可以看作一个可追加的日志（Log）文件，消息被追加到分区日志文件时，会分配一个特定的偏移量（offset），offset 是消息在分区中的唯一标识，Kafka 通过它来保证消息在分区内的顺序性。

  一个主题的分区可以分布在不同的服务器（broker）上，即一个主题可以横跨多个 broker。

  生产者发送的消息根据分区规则选择存储到哪一个分区。

  > 在创建主题时可以设置分区的个数，也可以在主题创建完成之后去修改分区的数量，通过增加分区的数量可以实现**水平扩展**。

## 2. 多副本机制

通过为 Kafka 集群中的每个分区设置多个副本，增强 Kafka 集群的容灾能力。

每个分区有一个 leader、多个 follower 副本，leader 副本负责处理此分区的所有读写请求，follower 副本负责同步 leader 副本的消息（在某一时刻，副本之间的消息可能存在差别）。当 leader 出现故障时，从 follower 副本中选举新的 leader 对外提供服务，从而实现故障的自动转移。

> Kafka 消费端也具备一定的容灾能力：消费者使用拉（Pull）模式从服务端拉取消息，同时会保存消息的具体位置，当消费者宕机后再次恢复上线时可以根据保存的位置重新拉取需要的消息。

### 1）AR/ISR/OSR

对于某个分区而言，将其所有副本称为 AR（Assigned Replicas），所有与 leader 保持一定程度同步的副本称为 ISR（In-Sync Replicas）。

> 注：由于所有的消息都是先发送到 leader，然后 follower 副本才能从 leader 中拉取消息进行同步，所以所有的 follower 副本都会存在一定程度的滞后。
>
> follower 副本相对于 leader 可忍受的滞后范围可以通过参数进行设置。

相对于 leader 同步滞后过多的副本组成OSR（Out-of-Sync Replicas），leader 负责维护和跟踪 ISR 集合中所有follower 副本的滞后状态，当 follower 副本落后太多或失效时，leader 会把它从 ISR 集合中剔除，如果 OSR 集合中有 follower 副本“追上”了 leader，leader 会把它从 OSR 集合移动至 ISR 集合。

> 默认情况下，当 leader 发生故障时，**只有在 ISR 集合中的副本才有资格被选举成为新的leader**。

### 2）HW/LEO

HW（High Watermark，高水位）：对应一个消息偏移量（offset），消费者只能拉取这个 offset 之前的消息。

LEO（Log End Offset）：当前日志文件中下一条待写入消息的 offset。

对于某个分区而言，其 ISR 集合中最小的 LEO 就是该分区的HW。









