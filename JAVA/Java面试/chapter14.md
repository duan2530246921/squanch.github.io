# RabbitMQ
## 135.rabbitmq 的使用场景有哪些？
---

<img src="/_markdownimg/rabbitmq1.png" alt="图片名称" align=center />

## 136.rabbitmq 有哪些重要的角色？
---

<img src="/_markdownimg/rabbitmq2.png" alt="图片名称" align=center />

## 137.rabbitmq 有哪些重要的组件？
---

<img src="/_markdownimg/rabbitmq3.png" alt="图片名称" align=center />

## 138.rabbitmq 中 vhost 的作用是什么？
---

<img src="/_markdownimg/rabbitmq4.png" alt="图片名称" align=center />

## 139.rabbitmq 的消息是怎么发送的？
---

<img src="/_markdownimg/rabbitmq5.png" alt="图片名称" align=center />

## 140.rabbitmq 怎么保证消息的稳定性？
---
- 提供了事务的功能。
- 通过将 channel 设置为 confirm（确认）模式。

## 141.rabbitmq 怎么避免消息丢失？
---
- 消息持久化
- ACK确认机制
- 设置集群镜像模式
- 消息补偿机制

## 142.要保证消息持久化成功的条件有哪些？
---
- 声明队列必须设置持久化 durable 设置为 true.
- 消息推送投递模式必须设置持久化，deliveryMode 设置为 2（持久）。
- 消息已经到达持久化交换器。
- 消息已经到达持久化队列。
 
以上四个条件都满足才能保证消息持久化成功。

## 143.rabbitmq 持久化有什么缺点？
---
- 持久化的缺地就是降低了服务器的吞吐量，
- 因为使用的是磁盘而非内存存储，
- 从而降低了吞吐量。可尽量使用 ssd 硬盘来缓解吞吐量的问题。

## 144.rabbitmq 有几种广播类型？
---
三种广播模式：
 
1. fanout: 所有bind到此exchange的queue都可以接收消息
         （纯广播，绑定到RabbitMQ的接受者都能收到消息）；
2. direct: 通过routingKey和exchange决定的那个唯一的queue可以接收消息；
3. topic:  所有符合routingKey(此时可以是一个表达式)的routingKey所bind的queue可以接收消息；

## 145.rabbitmq 怎么实现延迟消息队列？
---
- 通过消息过期后进入死信交换器，
- 再由交换器转发到延迟消费队列，实现延迟功能；
- 使用 RabbitMQ-delayed-message-exchange 插件实现延迟功能。

## 146.rabbitmq 集群有什么用？
---
集群主要有以下两个用途：
 
- 高可用：某个服务器出现问题，整个 RabbitMQ 还可以继续使用；
- 高容量：集群可以承载更多的消息量。

## 147.rabbitmq 节点的类型有哪些？
---
- 磁盘节点：消息会存储到磁盘。
- 内存节点：消息都存储在内存中，重启服务器消息丢失，性能高于磁盘类型

## 148.rabbitmq 集群搭建需要注意哪些问题？
---
- 各节点之间使用“--link”连接，此属性不能忽略。
     
- 各节点使用的 erlang cookie 值必须相同，此值相当于“秘钥”的功能，用于各节点的认证。
     
- 整个集群中必须包含一个磁盘节点。

## 149.rabbitmq 每个节点是其他节点的完整拷贝吗？为什么？
---
- 不是，原因有以下两个：
- 存储空间的考虑：如果每个节点都拥有所有队列的完全拷贝，
- 这样新增节点不但没有新增存储空间，反而增加了更多的冗余数据；
- 性能的考虑：如果每条消息都需要完整拷贝到每一个集群节点，
- 那新增节点并没有提升处理消息的能力，最多是保持和单节点相同的性能甚至是更糟。
    
## 150.rabbitmq 集群中唯一一个磁盘节点崩溃了会发生什么情况？
---
如果唯一磁盘的磁盘节点崩溃了，不能进行以下操作：
 
- 不能创建队列
- 不能创建交换器
- 不能创建绑定
- 不能添加用户
- 不能更改权限
- 不能添加和删除集群节点
 
唯一磁盘节点崩溃了，集群是可以保持运行的，但你不能更改任何东西

## 151.rabbitmq 对集群节点停止顺序有要求吗？
---
RabbitMQ 对集群的停止的顺序是有要求的，
应该先关闭内存节点，最后再关闭磁盘节点。如果顺序恰好相反的话，可能会造成消息的丢失。