# 1、Zookeeper

![](Zookeeper(一).assets/8.png)

Zookeeper是一个开放源代码的分布式协调服务。Zookeeper的设计目标是将那些复杂且容易出错的分布式一致性服务封装起来，构成一个高效可靠的原语集，并以一系列简单易用的接口提供给用户使用。

> Zookeeper顾名思义就是动物园管理员。因为Hadoop生态各个项目都是动物的图标。所以很符合管理员的形象。



## 1.1、概述

ZooKeeper 主要用来解决分布式集群中应用系统的一致性问题，它能提供基于类似于文件系统的目录节点树方式的数据存储。但是 ZooKeeper 并不是用来专门存储数据的，它的作用主要是用来**维护和监控存储数据的状态变化**。通过监控这些数据状态的变化，从而可以达到基于数据的集群管理。

- 很多大名鼎鼎的框架都基于 ZooKeeper 来实现分布式高可用，如：Dubbo、Kafka 等。
- Zookeeper就可以理解为是现实版的美团，我开了个饭店，如何才能让大家都能吃到我的饭菜呢？我入驻美团，这样大家就可以在美团中看到我的饭店，下订单，从而完成一次交易。
- Zookeeper(动物管理员)、Hadoop(大象)、Hive(蜜蜂)、Pig(猪)，大数据技术生态圈中都是采取动物的名字命名的，有趣吧！



## 1.2、工作机制

Zookeeper负责存储和管理大家都关心的数据：

1. Zookeeper 接受观察者的**注册**(可以理解为美团接受商家的入驻)，一旦数据发生变化(比如包子店突然5点打烊)，Zookeeper 就**通知**那些已经注册的观察者做出相应的反应



Zookeeper = 文件系统 + 通知机制

![](Zookeeper(一).assets/1.png)

1. 商家营业并入驻美团 - 将服务型应用程序注入到Zookeeper
2. 获取当前营业的饭店列表 - 客户端获得当前所有的服务型应用程序列表
3. 服务器节点下线 - Zookeeper监听某个服务型应用程序列表的变化
4. 服务器节点上下线事件通知 - Zookeeper将服务型应用程序的变化通知给客户端





## 1.3、特点

![](Zookeeper(一).assets/2.png)

1. 一个Leader和多个follower来组成的群狮。(众多服务器中，有一台服务器是头，其他服务器是随从)
2. 集群中只有有半数以上的节点存活，Zookeeper 就能正常工作。(5台服务器挂两台，没问题；4台服务器挂2台，就停止)
3. 全局数据一致性，每台服务器都保存一份相同的数据副本，无论 client 连接哪台 server，数据都是一致的(上面的孙悟空就是client，孙悟空1 向 follower1 服务器写一个数据，follower1 会将数据同步给其他 follower1)
4. 数据更新原子性，一次数据操作，要么成功，要么失败
5. 实时性，在一定时间范围内，client 能读取到最新数据
6. 客户端更新的请求按照顺序执行，会按照发送过来的顺序逐一执行(客户端发送123，执行123)

> 扩展：分布式和集群的区别？无论是分布式还是集群，都是很多人在做事情，具体区别如下：
>
> 例如：我有一个饭店，越来越火爆，我得多招聘一些工作人员
>
> - 分布式：招聘1个厨师，1个服务员，1个前台，**三个人负责的工作不一样**，但是最终目的都是为饭店工作
> - 集群：招聘3个服务员，**3个人的工作一样**



## 1.4、数据结构

![](Zookeeper(一).assets/3.png)

- ZooKeeper数据模型的结构与linux文件系统很类似，整体上可以看作是一棵树，每个节点称做一个ZNode(ZookeeperNode)。每一个ZNode默认能够存储1MB的数据（**元数据**），每个ZNode的路径都是唯一的
- 元数据(Metadata),又称**中介数据、中继数据**，为描述数据的数据(data about data),主要是描述数据属性(property)的信息，用来支持如指示存储位置、历史数据、资源查找、文件记录等功能。(可以理解存的不是数据，而是索引)



## 1.5、应用场景

提供的服务包括：**统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡**等



### 1.5.1、统一命名服务

- 在分布式环境下，通常需要对应用或者服务进行统一的命名，便于识别
- 例如：服务器的IP地址不容易记，但域名很容易记住

![](Zookeeper(一).assets/4.png)

例如拉钩教育有一个域名 `www.lagou.com`，但是这个域名下有很多机器，别人通过访问域名就可以访问到对应的IP，从下面的图可以看出。

![](Zookeeper(一).assets/5.png)





### 1.5.2、统一配置管理

- 分布式环境下，配置文件做同步是必经之路，1000台服务器，如果配置文件作出修改，那么必须能同步到这1000台服务器上。

![](Zookeeper(一).assets/6.png)

将配置管理交给Zookeeper：

1. 将配置信息写入到Zookeeper的某个节点上
2. 每个客户端都监听这个节点
3. 一旦节点中的数据文件被修改，Zookeeper就会通知给每台客户端





### 1.5.3、服务器节点动态上下线

- 客户端能实时获取服务器上下线的变化（在美团app上实时可以看到商家是否正在营业或打烊）

### 1.5.4、软负载均衡

- Zookeeper会记录每台服务器的访问数，让访问数最少的服务器去处理最新的客户端请求





## 1.6、CAP定理

![](Zookeeper(一).assets/7.png)

分布式系统正变得越来越重要，大型网站几乎都是分布式的。分布式系统的最大难点，就是各个节点的状态如何
同步。CAP定理是这方面的基本定理，也是理解分布式系统的起点.

分布式系统的三个指标：

- Consistency - 一致性
- Availability - 可用性
- Partition tolerance - 分区容错性

他们的第一个字母分别是C、A、P，这三个指标不可能同时做到。这个结论就叫做CAP定理。

### 1.6.1、分区容错性

大多数分布式系统都分布在多个子网络。每个子网络就叫做一个区。**分区容错的意思是，区间通信可能失败。**比
如，一台服务器放在中国，另一台服务器放在美国，这就是两个区，它们之间可能无法通信

- 结论：分区容错无避免，因此可以认为CAP的P总是成立。CAP定理告诉我们，剩下的C和A无法同时做到。



### 1.6.2、一致性

写操作之后的读操作，必须返回该值。举例来说，某条记录是v0，用户向 G1 发起一个写操作，将其改为v1，接下来用户的读操作就会得到v1，这就叫一致性。

问题是，用户有可能向G2发起读操作，由于G2的值没有发生变化，因此返回的是v0。G1和G2读操作的结果不一致，就不满足一致性了。为了让G2也能变为v1，就要在G1写操作的时候，让G1向G2发送一条消息，要求G2也改成v1



### 1.6.3、可用性

可用性：访问你的服务，你响应的速度快不快。



### 1.6.4、一致性和可用性的矛盾

如果保证G2的一致性，那么G1必须在写操作时，锁定G2的读操作和写操作。只有数据同步好，才能重新开放读写。锁定期间，G2不能读写，没有可用性。





### 1.6.5、一致性和可用性如何选择

- 一致性：特别是涉及到重要的数据，比如钱、商品数量、商品价格，要保证一致性
- 可用性：网站的更新不是特别强调一致性，短时期内，一些用户拿到老版本，一些用户拿到新版本，问题不会特别大





















## 1.7、Zookeeper基本概念

Zookeeper 已经得到了广泛的应用。诸如 Hadoop、HBase、Storm、Kafka 等越来越多的大型分布式项目都将 Zookeeper 作为核心组件。

![](Zookeeper(一).assets/9.png)



### 1.7.1、集群角色

通常在分布式系统中，构成一个集群的每一台机器都有自己的角色，最典型的集群模式就是Master/Slave模式(**主备模式**)。在这种模式中，我们把能够处理所有写操作的机器称为Master机器，把所有通过异步复制方式获取最新数据，并提供读服务的机器称为Slave机器。

- 而在ZooKeeper中，这些概念被颠覆了。它没有沿用传统的Master/Slave概念，而是引入了Leader、Follower和Observer三种角色。领导者和随从者模式。

![](Zookeeper(一).assets/10.png)



### 1.7.2、数据节点

在谈到分布式的时候，我们通常所说的节点是指组成集群的每一台机器。

![](Zookeeper(一).assets/11.png)

但是在Zookeeper中节点分为两类：

1. 第一类是指构成集群的机器，我们称之为机器节点。
2. 第二类是指数据模型中的数据单元，我们称之为数据节点 - ZNode
3. Zookeeper 将所有数据存储在内存中，数据模型是一棵树



### 1.7.3、Watch监听机制

![](Zookeeper(一).assets/12.png)

ZooKeeper允许用户在指定节点上注册一些Watcher,并且在一些特定事件触发的时候，ZooKeeper服务端会将事件通知到感兴趣的客户端上去，该机制是ZooKeeper实现分布式协调服务的重要特性。





### 1.7.4、ACL权限控制

ZooKeeper采用ACL(Access Control Lists)策略来进行权限控制，类似于UNIX文件系统的权限控制。ZooKeeper定义了如下5种权限。

- CREATE:创建子节点的权限
- READ:获取节点数据和子节点列表的权限
- WRITE:更新节点数据的权限
- DELETE:删除子节点的权限
- ADMIN:设置节点ACL的权限

注意：create和delete这两种权限都是针对子书点的权限控制。





# 2、安装

ZooKeeper服务器是用Java创建的，它运行在JVM之上。需要安装JDK 7或更高版本。

## 2.1、JDK17

我直接使用宝塔面板安装 jdk管理器，在里面安装了 jdk17，缺点是需要自己手动设置环境变量。

1. `vim /etc/profile`,`shift+g`切换到最后一行，按`i`切换编辑模式，将下方代码粘贴进去`:wq`保存

```bash
export JAVA_HOME=/www/server/java/jdk-17.0.8
export PATH=$JAVA_HOME/bin:$PATH
```

2. `source /etc/profile `让新的环境变量生效
3. `echo $PATH` 查看环境变量
4. `javac -version` 查看



## 2.2、Zookeeper

- [ZooKeeper官网](https://zookeeper.apache.org/releases.html)

![](Zookeeper(一).assets/13.png)

![](Zookeeper(一).assets/14.png)



2. 由于我是使用了宝塔，宝塔面板默认的安装位置是在`www/service`下，所以我在`www/service`下创建了`zookeeper`文件夹，将下载的安装包上传

![](Zookeeper(一).assets/15.png)



3. 解压

```bash
# 解压
tar -zxvf apache-zookeeper-3.8.4-bin.tar.gz

# 进入目录
cd apache-zookeeper-3.8.4-bin

# 进入conf目录
cd conf

# 拷贝配置文件备用(拷贝  zoo_sample.cfg 并起名为 zoo.cfg)
cp  zoo_sample.cfg  zoo.cfg
```

4. 在`/www/server/zookeeper`下新建文件夹`zkdata`、`zklogs`

![](Zookeeper(一).assets/16.png)



5. 修改`zoo.cfg`

```bash
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/www/server/zookeeper/zkdata
dataLogDir=/www/server/zookeeper/zklogs
clientPort=2181
```

- `tickTime` 定义了 ZooKeeper 服务器之间进行通信的基本时间单位（毫秒）
- `initLimit` 表示在启动阶段，follower 与 leader 之间的最大容许延迟，以 `tickTime` 为单位。例如，如果 `tickTime` 是 2000 毫秒 (2 秒)，那么 `initLimit` 设置为 10 就意味着 follower 必须在 20 秒内与 leader 同步初始化数据。
- `syncLimit` 定义了在正常操作期间，follower 与 leader 之间最大的容许延迟，同样以 `tickTime` 为单位。在这个例子中，如果 `tickTime` 是 2 秒，那么 `syncLimit` 设置为 5 就意味着 follower 必须在 10 秒内与 leader 同步数据。
- `dataDir` 指定了 ZooKeeper 存储快照和事务日志的目录。
- `dataLogDir` 指定 ZooKeeper 存储事务日志的位置。
- `clientPort` 定义了客户端连接到 ZooKeeper 服务器所使用的端口。



6. 进入 bin 目录启动

```bash
./zkServer.sh  start
```

![](Zookeeper(一).assets/17.png)



7. 查看zookeeper的状态

```bash
./zkServer.sh status
```

![](Zookeeper(一).assets/18.png)







8. 加入环境变量

   `vim /etc/profile`,`shift+g`切换到最后一行，按`i`切换编辑模式，将下方代码粘贴进去`:wq`保存

   `source /etc/profile `让新的环境变量生效

```bash
export ZOOKEEPER_HOME=/www/server/zookeeper/apache-zookeeper-3.8.4-bin
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```



9. 这样就可以随时随地启动和连接了

```bash
# 启动
zkServer.sh start

# 连接
zkCli.sh

# 退出
quit
```



# 3、Zookeeper节点特性

![](Zookeeper(一).assets/19.png)

ZooKeeper节点是有生命周期的，这取决于节点的类型。节点类型可以分为持久节点、临时节点，以及时序节
点，具体在节点创建过程中，一般是组合使用，可以生成以下4种节点类型。

## 3.1、持久节点

持久节点是zookeeper中最常见的一种节点类型。所谓持久节点，是指改数据节点被创建后，就会一直存于与zookeeper服务器上，直到有删除操作来主动清除这个节点。



## 3.2、持久顺序节点

这类节点的基本特性和上面的节点类型是一致的。额外的特性是，在ZK中，每个父节点会为他的第一级子节点维护一份时序，会记录每个子节点创建的先后顺序。

![](Zookeeper(一).assets/20.png)





## 3.3、临时节点

所谓临时性是指，如果将节点创建为临时节点，那么该节点数据不会一直存储在ZooKeeper服务器上。

- 和持久节点不同的是，临时节点的生命周期和客户端会话绑定。也就是说，如果客户端会话失效，那么这个节点就会自动被清除掉。注意，这里提到的是会话失效，而非连接断开。另外，在临时节点下面不能创建子节点。



## 3.4、临时顺序节点

临时顺序节点的基本特性和临时节点是一致的，同样是在临时节点的基础上，添加了顺序的特性。





# 4、Zookeeper节点操作命令

![](Zookeeper(一).assets/21.png)







## 4.1、创建ZK节点

语法结构：

```bash
create [-s] [-e] path data acl
```

- -s：顺序节点
- -e：临时节点
- 默认情况下，不添加-s或者-e参数，创建的是持久节点

```bash
create -e -s /mysql mysql
```

> 注意首先使用`./zkCli.sh`进行客户端连接

![](Zookeeper(一).assets/22.png)



> 第一次部署的ZooKeeper集群，默认在根节点“"/"下面有一个叫作/zookeeper的保留节点







## 4.2、读取ZK节点

语法结构：

```bash
ls path [watch]
```

示例:

```bash
# 查看根路径下的节点
ls /

# 查看 /java 路径下的节点
ls /java
```



当然get命令也可以获取zookeeper指定节点的数据内容和属性信息

语法结构：

```bash
get path [watch]
```

示例:

```bash
get /java
```





## 4.3、修改ZK节点

语法结构：

```bash
set path  data [version]
```

示例:

```bash
set /java java
```

> data就是要更新的新内容。注意，set命令后面还有一个version参数，在ZooKeeper中，节点的数据是有版本概念的，这个参数用于指定本次更新操作是基于ZNode的哪一个数据版本进行的。







## 4.4、删除ZK节点

语法结构：

```bash
delete path   [version]
```

示例:

```bash
delete /java
```

> 如果节点包含子节点就报错



## 4.5、查看ZK节点数据信息

![](Zookeeper(一).assets/23.png)

每个节点都有自己的状态信息，就很像每个人的身份信息一样。

语法结构：

```bash
stat  /java
```

示例:

```bash
stat  /java
```

结果：

```bash
cZxid = 0x6
ctime = Mon Aug 12 12:04:23 CST 2024
mZxid = 0x6
mtime = Mon Aug 12 12:04:23 CST 2024
pZxid = 0x6
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
```

![](Zookeeper(一).assets/24.png)



# 5、Zookeeper Watch监听机制

![](Zookeeper(一).assets/25.png)

ZooKeeper提供了分布式数据的发布/订阅功能。一个典型的发布/订阅模型系统定义了一种一对多的订阅关系，能够让多个订阅者同时监听某一个主题对象，当这个主题对象自身状态变化时，会通知所有订阅者，使它们能够做出相应的处理。

> 注意：在ZooKeeper中，引入了Watcher机制来实现这种分布式的通知功能。ZooKeeper允许客户端向服务端注册一个Watcher监听，当服务端的一些指定事件触发了这个Watcher,那么就会向指定客户端发送一个事件通知来实现分布式的通知功能。



## 5.1、监听节点变化

语法结构：

```bash
ls -w path
```

**命令如果使用watch,那么监听的是节点的变化，而不是值的变化**。



## 5.2、监听节点值的变化

语法结构：

```bash
get -w path
```

> watch监听机制只能够使用一次，如果下次想要使用，必须重新监听，就比如ls path watch命令，只能监听节点路径的改变一次，如果还想监听，那么需要再执行一次ls path watchi命令。





## 5.3、Watcher特性总结

**一次性**：无论是服务端还是客户端，一旦一个Watcher被触发，ZooKeeper都会将其从相应的存储中移除。因此，在Watcher的使用上，需要反复注册。这样的设计有效地减轻了服务端的压力。







# 6、Zookeeper权限控制ACL

![](Zookeeper(一).assets/26.png)



在ZooKeeperl的实际使用中，我们的做法往往是搭建一个共用的ZooKeeper集群，统一为若干个应用提供服务。在这种情况下，不同的应用之间往往是不会存在共享数据的使用场景的，因此需要解决不同应用之间的权限问题。



ACL权限控制：主要包含三部分

1. 权限模式 Schema
2. 授权对象 ID
3. 权限 Permission

> - ZooKeeper的权限控制是基于每个znode节点的，需要对每个节点设置权限
> - 每个znode支持设置多种权限控制方案和多个权限
> - 子节点不会继承父节点的权限，客户端无权访问某节点，但可能可以访问它的子节点

例子：

```bash
setAcl /java ip:128.0.0.1:crwda
```

![](Zookeeper(一).assets/27.png)





## 6.1、权限模式Schema

Zookeeper内置了一些权限控制方案，可以用以下方案为每个节点设置权限：

| 方案   | 描述                                 |
| ------ | ------------------------------------ |
| world  | 只有一个用户:anyone,代表所有人(默认) |
| ip     | 使用ip地址认证                       |
| auth   | 使用已添加认证的用户认证             |
| digest | 使用`用户名:密码`方式认证            |

![](Zookeeper(一).assets/28.png)



## 6.2、授权对象ID

授权对象ID是指，权限赋予的用户或者一个实体，例如：IP地址或者机器。授权模式schema与授权对象D之间关系：

| 权限模式 | 授权对象                                                 |
| -------- | -------------------------------------------------------- |
| IP       | 通常是一个IP地址或者IP段                                 |
| Digest   | 自定义,通常是"username:BASE64(SHA-1(username:password))" |
| World    | 只有一个ID:"anyone"                                      |
| Super    | 与Digest模式一致                                         |

![](Zookeeper(一).assets/29.png)





## 6.3、权限Permission

| 权限   | ACL简写 | 描述                             |
| ------ | ------- | -------------------------------- |
| create | c       | 可以创建子节点                   |
| delete | d       | 可以删除子节点(仅下一级节点)     |
| read   | r       | 可以读取节点数据及显示子节点列表 |
| write  | w       | 可以设置节点数据                 |
| admin  | a       | 可以设置节点访问控制列表权限     |



## 6.4、权限相关命令

| 命令    | 使用方式 | 描述         |
| ------- | -------- | ------------ |
| getAcl  | getAcl   | 读取ACL权限  |
| setAcl  | setAcl   | 设置ACL权限  |
| addauth | addauth  | 添加认证用户 |



## 6.5、权限实战

### 6.5.1、World方案

语法：

```bash
setAcl <path> world:anyone:<acl>
```

![](Zookeeper(一).assets/30.png)



### 6.5.2、IP方案

语法：

```bash
setAcl <path> ip:<ip>:<acl>
```

示例：

```bash
#以指定的ip连接
./zkCli.sh -server 192.168.126.132:2181

# 创建节点并指定ip权限
create /node node
setAcl /node ip:192.168.126.132:cdrwa

# 读取ACL权限
getAcl /node
```

![](Zookeeper(一).assets/31.png)

> 假如我们使用localhost进行连接，会拿不到`/node`下的值的，因为没有权限
>
> ![](Zookeeper(一).assets/32.png)





### 6.5.3、Auth方案

语法：

```bash
setAcl <path> auth:<user>:<acl>
```



首先先添加认证用户：

```bash
addauth digest <user>:<password>
```









### 6.5.4、Digest方案



语法：

```bash
setAcl <path> digest:<user><password><acl>
```

这里的密码要求是经过SHA1及BASE64处理的密文。





# 7、Zookeeper选举机制

![](Zookeeper(一).assets/33.png)

如何判断Zookeeper哪台服务器作为Leader领导呢？

- Zookeeper集群中只有超过半数以上的服务器启动，集群才能正常工作；
- 在集群正常工作之前，myid小的服务器给myid大的服务器投票，直到集群正常工作，选出Leader;
- 半数机制；

![](Zookeeper(一).assets/34.png)

> 服务器之间进行投票选举出Leader































