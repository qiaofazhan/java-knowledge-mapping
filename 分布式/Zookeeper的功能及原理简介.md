# Zookeeper的功能及原理
# 前言
最近在做sweet框架——一个基于spring boot开发、支持分布式架构的基础架构，其服务注册与发现中心是用zookeeper来实现的，我一直也在用，今天特此整理出来
# 1 Zookeeper是什么
ZooKeeper是一个分布式的，开源的分布式应用程序协调服务，是Google的Chubby一个开源的实现，它是集群的管理者，监视着集群中各个节点的状态根据节点提交的反馈进行下一步合理操作。
## 1.1 Zookeeper提供了什么功能
+ 文件系统
每个子目录都被称为znode，和文件系统一样，我们可以自由的增加和删除，在一个znode下增加和删除znode，znode也可以存储数据。znode有四种类型：

1、PERSISTENT：持久化目录节点。客户端与zookeeper的连接断开后，该节点依旧存在

2、PERSISTENT_SEQUENTIAL：持久化顺序编号目录节点。客户端与zookeeper连接断开后，该节点依旧存在，只是Zookeeper给该节点名末尾会自动追加一个10位数的单调递增的序号。

3、EPHEMERAL：临时目录节点。客户端与zookeeper的连接断开后，该节点自动被删除，临时节点不允许有子节点。

4、EPHEMRAL_SEQUENTIAL：临时目录节点。只是Zookeeper给该节点名末尾会自动追加一个10位数的单调递增的序号

+ 通知机制
客户端注册监听它关心的目录节点，当目录节点发生变化（数据改变、被删除、子目录节点增加/删除等）时，zookeeper会通知客户端。

## 1.2 Zookeeper提供了哪些功能
Zookeeper提供了**命名服务、配置管理、集群管理、分布式锁和队列管理**

1、命名服务

在Zookeeper的文件系统里创建一个目录，即有唯一的path。

2、配置管理

如果程序分散部署在多台机器上，要逐个改变配置还是比较困难的。现在把这些配置全部放到zookeeper上去，保存在 Zookeeper 的某个目录节点中，然后所有相关应用程序对这个目录节点进行监听，一旦配置信息发生变化，每个应用程序就会收到 Zookeeper 的通知，然后从 Zookeeper 获取新的配置信息应用到系统中就好了

3、集群管理
在一台机器上运营一个ZooKeeper实例，称之为单机（Standalone）模式。单机模式有个致命的缺陷，一旦唯一的实例挂了，依赖ZooKeeper的应用全得完蛋。

实际应用当中，一般都是采用集群模式来部署ZooKeeper，集群中的Server为奇数（2N+1）。只要集群中的多数（大于N+1台）Server活着，集群就能对外提供服务。

在每台机器上部署一个ZooKeeper实例，多台机器组成集群，称之为完全分布式集群。此外，还可以在仅有的一台机器上部署多个ZooKeeper实例，以伪集群模式运行。

集群配置，主要是在zk.cfg文件进行配置

**集群管理功能**：是否有机器退出/加入、选举master

+ 是否有机器退出：所有机器约定在父目录下创建临时目录节点，然后监听父目录节点变化消息。一般有机器挂掉，该机器与zookeeper的连接断开，如果session超时，则其所创建的临时节点被删除，所有其他机器都收到通知。
+ 是否有机器加入：所有机器收到通知，有节点接入





