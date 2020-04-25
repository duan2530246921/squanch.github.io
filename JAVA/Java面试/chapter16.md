# Zookeeper
## 157.zookeeper 是什么？
---
ZooKeeper由雅虎研究院开发，是Google Chubby的开源实现，后来托管到Apache，于2010年11月正式成为Apache的顶级项目。
ZooKeeper是一个经典的分布式数据一致性解决方案，致力于为分布式应用提供一个高性能、高可用，且具有严格顺序访问控制能力的分布式协调服务。
分布式应用程序可以基于ZooKeeper实现数据发布与订阅、负载均衡、命名服务、分布式协调与通知、集群管理、Leader选举、分布式锁、分布式队列等功能。

<img src="/_markdownimg/zookeeper.png" alt="图片名称" align=center />

## 158.zookeeper 都有哪些功能？
---

<img src="/_markdownimg/zookeeper2.png" alt="图片名称" align=center />

## 159.zookeeper 有几种部署模式？
---

<img src="/_markdownimg/zookeeper3.png" alt="图片名称" align=center />

## 160.zookeeper 怎么保证主从节点的状态同步？
---

<img src="/_markdownimg/zookeeper4.png" alt="图片名称" align=center />

## 161.集群中为什么要有主节点？
---
- 在分布式环境中，有些业务逻辑只需要集群中的某一台机器进行执行，
 
- 其他的机器可以共享这个结果，这样可以大大减少重复计算，提高性能，所以就需要主节点。

## 162.集群中有 3 台服务器，其中一个节点宕机，这个时候 zookeeper 还可以使用吗？
---
可以继续使用，单数服务器只要没超过一半的服务器宕机就可以继续使用。

## 163.说一下 zookeeper 的通知机制？
---
- 客户端端会对某个 znode 建立一个 watcher 事件，
 
- 当该 znode 发生变化时，这些客户端会收到 zookeeper 的通知，
 
- 然后客户端可以根据 znode 变化来做出业务上的改变。