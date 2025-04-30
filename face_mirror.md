[toc]

# 序

​	对，没错，就叫做“面镜”，不是面经而是一面镜子，一面让人原形毕露的镜子。学海无涯苦作舟，想必这个学字深入人心，作为一名后端开发人员，需要掌握的东西可太多了，最麻烦的就是无法系统地学习各种知识。现在好了，不用东拼西凑了，更不用付费阅读了，都在这了。这面镜子是我自己的“照妖镜”，照出瑕疵，也希望能够帮助各位，在升职加薪的路上能够披荆斩棘，一路向前。内容错误处，望请批正。

-- 猫鱼儿

# 一、数据库理论

业务系统离不开数据的存储与查询。

## 1、MongoDB

### 1-1、基本概念

集合（表）、文档（记录）、字段（字段）

### 1-2、存储引擎与索引

3.2版本后，使用WiredTiger存储引擎，B+树结构，存储单元为page，分为**root page、internal page、leaf page**，每一个节点是一个page，非叶子节点为索引节点，数据存储在叶子节点上

```mermaid
graph TD
    A[root page] --> B(internal page)
    A[root page] --> D(internal page)
    A[root page] --> E(internal page)
    D --> C[leaf page]
  
```

索引：

单键值索引、

复合索引（联合索引）、

多键索引（一个字段下可能是一个数组，存在多键情况）

哈希索引：顾名思义，字段哈希值索引

文本索引：内容检索，全文索引，一般通过一个或者多个字符串对文档内容进行索引

### 1-3、分片复制集群

三个组件router server、config server、sharding server。

router server：接受前段连接请求，然后根据config server进行路由转发

config server：存储元数据信息，不存具体业务数据

sharding server（主从结构）：数据实际储存位置，多主多从，高可用高并发

类似Redis的集群分片

## 2、Redis

内存型数据库，c语言编写，旨在加速查存

### 2-1、基本数据类型剖析

​	Redis使用c语言编写，封装了八种基本数据类型，string、set、zset、list、hash、hyperloglog、geospatial、，这八种数据结构基本满足需求。

#### 2-1-1、String

一、基础特性

1. ‌**二进制安全存储**‌

   - 可存储任意格式数据（包括序列化对象、图片二进制等）
   - 最大容量512MB，支持文本、数字、二进制混合内容

2. ‌**底层实现**‌

   - 采用简单动态字符串（SDS）结构，包含：

     ```c
     struct sdshdr {
         int len;    // 已用字节数
         int free;   // 剩余空间
         char buf[]; // 实际数据存储
     };
     ```

   - 预分配空间减少内存重分配次数

------

二、核心操作命令

| 命令类型     | 示例                   | 作用                     |
| ------------ | ---------------------- | ------------------------ |
| ‌**基础操作**‌ | `SET key value`        | 设置键值对               |
|              | `GET key`              | 获取值（不存在返回nil）  |
| ‌**数字运算**‌ | `INCR key`             | 值自增1（原子操作）      |
|              | `INCRBY key increment` | 指定步长自增             |
| ‌**位操作**‌   | `SETBIT key offset 1`  | 设置二进制位（支持位图） |
|              | `BITCOUNT key`         | 统计值为1的位数          |
| ‌**批量操作**‌ | `MSET k1 v1 k2 v2`     | 批量设置键值             |

------

三、高级特性

1. ‌**动态扩展**‌
   - 空间不足时自动扩容（小于1MB时加倍，大于1MB时每次扩1MB）
2. ‌**内存优化**‌
   - 对短字符串（≤39字节）采用embstr编码，减少内存碎片
   - 长字符串使用raw编码，独立分配内存
3. ‌**过期控制**‌
   - 支持`SETEX key seconds value`设置过期时间
   - 结合惰性删除+定期删除机制清理过期键

------

四、典型应用场景

1. ‌**缓存加速**‌
   - 存储热点数据（如商品详情）
2. ‌**计数器**‌
   - 利用`INCR`实现阅读量/点赞统计
3. ‌**分布式锁**‌
   - 通过`SETNX`实现互斥锁
4. ‌**位图处理**‌
   - 用户签到记录（每日1bit）

> 注：String类型在Redis 5.0后支持更紧凑的存储格式优化

#### 2-1-2、Set

一、核心特性

1. ‌**数据结构本质**‌

   - 无序且唯一的字符串集合，底层通过哈希表实现
   - 最大可存储2³²-1个元素（约40亿）
   - 所有操作时间复杂度为O(1)

2. ‌**底层编码**‌

   | 编码类型    | 触发条件                 | 特点                   |
   | ----------- | ------------------------ | ---------------------- |
   | `intset`    | 元素均为整数且数量≤5123  | 连续内存存储，节省空间 |
   | `hashtable` | 元素含非整数或数量＞5123 | 标准哈希表结构         |

------

二、核心操作命令

| ‌**命令组**‌   | ‌**示例**‌              | ‌**作用**‌             |
| ------------ | --------------------- | -------------------- |
| ‌**元素操作**‌ | `SADD myset "A" "B"`  | 添加元素（自动去重） |
|              | `SREM myset "A"`      | 删除指定元素         |
| ‌**查询操作**‌ | `SMEMBERS myset`      | 获取所有元素（无序） |
|              | `SISMEMBER myset "B"` | 检查元素是否存在     |
| ‌**集合运算**‌ | `SINTER set1 set2`    | 返回交集             |
|              | `SUNION set1 set2`    | 返回并集             |
| ‌**随机操作**‌ | `SPOP myset`          | 随机移除并返回元素   |

------

三、典型应用场景

1. ‌**去重存储**‌
   - 专利中的物流单号去重（动态选择Set或Bitmap）
   - 用户标签系统存储
2. ‌**关系运算**‌
   - 共同好友推荐（SINTER）
   - 商品品类筛选组合
3. ‌**随机抽奖**‌
   - 通过SPOP实现公平随机选取

------

四、性能优化建议

1. ‌**控制元素类型**‌
   - 尽量使用整数元素以触发intset编码
2. ‌**避免大Key**‌
   - 元素超过1万时考虑分片存储（如`set:part1`/`set:part2`）
3. ‌**替代方案选择**‌
   - 超大数据量（亿级）可结合Bitmap优化内存

> 注：Set与Zset的主要区别在于是否维护元素顺序

#### 2-1-3、Zset

一、核心特性

1. ‌**数据结构本质**‌
   - 在Set基础上增加score排序维度，元素唯一但score可重复
   - 默认按score升序排列，score相同时按字典序排序
2. ‌**底层实现演进**‌
   - ‌Redis 7.0前：
     - 小数据量（元素<128且单元素<64B）使用Ziplist，大数据量用跳表（Skiplist）
   - ‌Redis 7.0+：
     - Listpack完全替代Ziplist，仍与跳表组合使用

二、底层结构详解

| 结构类型     | 适用场景         | 时间复杂度              | 内存效率           |
| ------------ | ---------------- | ----------------------- | ------------------ |
| ‌**Listpack**‌ | 元素少且体积小   | 插入/删除O(N)           | 极高（连续存储）   |
| ‌**跳表**‌     | 大数据量或大元素 | 插入/删除/查询均O(logN) | 中等（需存储指针） |

‌**跳表特殊设计**‌：

- 多层索引结构，最高32层，每层以1/4概率升级
- 节点同时存储score和member，支持O(1)分数查询

三、典型应用场景

1. ‌实时排行榜
   - 通过`ZREVRANGE`获取TOP N，`ZINCRBY`动态更新分数
2. ‌延时队列
   - 以时间戳为score，定时扫描到期任务
3. ‌滑动窗口限流
   - 用ZSet存储请求时间戳，通过`ZREMRANGEBYSCORE`维护窗口

四、性能优化建议

1. ‌参数调优
   - 调整`zset-max-listpack-entries`（默认128）和`zset-max-listpack-value`（默认64）控制结构转换阈值
2. ‌批量操作
   - 使用`ZADD`批量插入替代循环单次插入，减少内存重分配次数

当前版本（Redis 7.0+）中，ZSet在百万级数据量下仍能保持99%操作性能在10ms内

#### 2-1-4、List

一、核心特性

1. ‌**数据结构本质**‌

   - 有序的字符串元素集合，支持重复值，最大容量2³²-1个元素
   - 支持双向操作（头插/尾插），时间复杂度O(1)

2. ‌**底层实现演进**‌

   | 版本        | 数据结构                      | 特点                                    |
   | ----------- | ----------------------------- | --------------------------------------- |
   | Redis 3.2前 | ziplist + linkedlist          | 小数据用压缩列表，大数据转双向链表      |
   | Redis 3.2+  | quicklist                     | 双向链表嵌套ziplist节点，平衡内存与性能 |
   | Redis 7.0   | quicklist+listpack替代ziplist | 解决连锁更新问题                        |

二、操作命令

1. ‌**基础操作**‌
   - `LPUSH/RPUSH`：头插/尾插元素（批量操作时间复杂度O(N)）
   - `LPOP/RPOP`：非阻塞式弹出元素，`BLPOP/BRPOP`为阻塞版本
   - `LRANGE`：获取区间元素（左闭右闭），时间复杂度O(S+N)
2. ‌**特殊场景命令**‌
   - `LTRIM`：裁剪列表，仅保留指定区间
   - `RPOPLPUSH`：原子化移动元素到另一列表

三、应用场景

1. ‌**消息队列**‌
   - 组合`LPUSH+BRPOP`实现生产者-消费者模型
   - 相比Pub/Sub模式可持久化消息
2. ‌**栈结构**‌
   - `LPUSH+LPOP`实现后进先出
3. ‌**实时排行榜**‌
   - 通过`LRANGE`快速获取最新N条数据

四、性能优化点

1. ‌**内存控制**‌
   - quicklist默认单个ziplist节点8KB，可通过`list-max-ziplist-size`调整
   - 大元素建议拆分存储避免节点膨胀
2. ‌**阻塞操作注意**‌
   - `BLPOP`多key时按顺序检查，超时设置需谨慎

当前最新版本(2025年)推荐使用quicklist+listpack组合，在内存效率和操作性能间取得最佳平衡



#### 2-1-5、Hash

一、数据结构特性

1. ‌**存储结构**‌

   - 采用`field-value`嵌套映射，形如`key={ {field1,value1},...,{fieldN,valueN} }`
   - 每个Hash最多可存储2³²-1个键值对（约40亿）

2. ‌**底层实现**‌

   | 编码类型    | 触发条件                    | 特点                   |
   | ----------- | --------------------------- | ---------------------- |
   | `ziplist`   | 字段数＜512且值长度＜64字节 | 内存连续存储，节省空间 |
   | `hashtable` | 超出ziplist限制时           | 查询时间复杂度O(1)     |

------

二、核心操作命令

| ‌**命令组**‌   | ‌**示例**‌                              | ‌**作用**‌       |
| ------------ | ------------------------------------- | -------------- |
| ‌**字段操作**‌ | `HSET user:1 name "John"`             | 设置单个字段   |
|              | `HGET user:1 name`                    | 获取字段值     |
| ‌**批量操作**‌ | `HMSET product:100 price 99 stock 50` | 批量设置字段   |
|              | `HMGET product:100 price stock`       | 批量获取字段   |
| ‌**数字运算**‌ | `HINCRBY user:1 age 1`                | 字段值原子递增 |
| ‌**扩展操作**‌ | `HSCAN user:1 0 MATCH *name*`         | 增量迭代字段   |

------

三、应用场景

1. ‌对象存储
   - 存储用户信息（如`user:id`包含name/age/email字段）
2. ‌商品属性
   - 商品ID关联多维度属性（价格/库存/规格）
3. ‌聚合统计
   - 通过`HINCRBY`实现实时计数器

------

四、性能优化建议

1. ‌控制字段数量
   - 避免单个Hash超过500字段，防止ziplist转hashtable
2. ‌大Key拆分
   - 字段过多时可按业务维度拆分（如`user:1:base`和`user:1:ext`）
3. ‌集群分片
   - 在Redis Cluster中，Hash整体存储在单个分片

> 注：Hash类型相比String更节省内存（存储相同字段时减少key元数据开销）

### 2-2、键的管理

#### 2-2-1、淘汰策略

内存快满了决定哪些key需要被淘汰，为新键腾空间

1、默认不淘汰 noevication，内存满了，执行拒绝策略

2、设置了key的超时时间的

​		1、volitile-lru：最近最少使用优先淘汰

​		2、volitile-lfu：最少使用频率的优先淘汰

​		3、volitile-random：从设置了超时时间的里面随机选一个

​		4、volitile-ttl：超时时间剩余最少的key优先（快要过期的）

3、全局键空间

​		1、all-lru：最近最少使用的优先淘汰

​		2、all-lfu：最少使用的

​		3、all-random：随机淘汰

4、策略选择建议

1. ‌**缓存系统**‌：优先选择`allkeys-lru`（存在热点数据）或`allkeys-lfu`（访问频率差异大）
2. ‌**临时数据**‌：使用`volatile-ttl`快速清理过期数据，或`volatile-lru`保留常用临时数据
3. ‌**配置方法**‌：在redis.conf中设置`maxmemory-policy`参数

> 注：LRU/LFU算法在Redis中采用近似实现，通过随机采样而非全量统计

#### 2-2-2、过期策略

这个仅针对设置了过期时间的key

​		1、惰性删除

设置了过期时间，仅在客户端访问key时去检测是否超时

好处，对服务器的影响较小。

​		2、定期删除

服务器内部定时器定时隔100ms去随机采样一批20个键，若过期率达20%，重复扫描直至低于20%

需要额外线程资源处理

### 2-3、Redis的执行流程

基础流程

```mermaid
%% 7.0版本之后引入IO多线程提高并发能力，但仍然保留命令执行时的单线程
graph TB
    A[客户端请求] --> B{多线程网络I/O}
    B --> C[线程1:Socket读取]
    B --> D[线程2:协议解析]
    B --> E[线程3:数据缓冲]
    C & D & E --> F[单线程命令执行]
    F --> G[多线程结果回写]
    
    

```



#### 2-3-1 、6.0版本的执行流程

一、核心执行模型

1. ‌**单线程事件循环**‌
   - 采用Reactor模式处理I/O事件，主线程通过`aeEventLoop`实现事件分发
   - 单线程避免锁竞争，命令执行平均耗时0.1ms级
2. ‌**I/O多路复用机制**‌
   - 封装epoll/kqueue等系统调用，单线程可处理10万级并发连接
   - 事件处理器分为：连接应答处理器/命令请求处理器/命令回复处理器

二、命令执行全流程

```mermaid
sequenceDiagram
    participant Client
    participant Socket
    participant RedisCore
    Client->>Socket: 发送RESP协议命令
    Socket->>RedisCore: 通过文件事件触发读取
    RedisCore->>RedisCore: 解析命令→查找对应函数→执行
    RedisCore->>Socket: 写入响应结果
    Socket->>Client: 返回执行结果
```

1. ‌**请求处理阶段**‌
   - 客户端命令转换为RESP协议格式传输
   - `redisClient`结构体存储连接状态和命令参数
2. ‌**命令执行阶段**‌
   - 通过`redisCommand`结构体查找对应执行函数
   - 执行过程严格单线程化，保障Lua脚本/事务的原子性

三、关键技术实现

1. ‌**内存管理**‌
   - 使用jemalloc分配器减少内存碎片
   - 渐进式rehash解决哈希表扩容阻塞问题
2. ‌**持久化机制**‌
   - RDB快照采用COW(Copy-On-Write)技术生成
   - AOF日志通过`fsync`策略控制数据安全级别

四、性能优化设计

| 设计要点   | 实现方式                   | 效果提升         |
| ---------- | -------------------------- | ---------------- |
| 网络通信   | I/O多路复用+非阻塞Socket   | 连接数突破10万级 |
| 数据结构   | SDS动态字符串+压缩列表优化 | 内存节省40%      |
| 过期键清理 | 惰性删除+定期删除双策略    | CPU占用降低35%   |

> 注：Redis 7.0在保留核心单线程模型基础上，新增多线程网络I/O处理能力

#### 2-3-2 、7.0以及之后的执行优化

一、核心执行流程（多线程I/O增强版）

```mermaid
sequenceDiagram
    participant Client
    participant ThreadPool
    participant MainThread
    participant DataEngine
    Client->>ThreadPool: 发送RESP3协议请求
    ThreadPool->>MainThread: 批量解析命令→入队执行队列
    MainThread->>DataEngine: 单线程原子性执行命令
    DataEngine-->>MainThread: 返回执行结果
    MainThread->>ThreadPool: 写入动态缓冲区
    ThreadPool->>Client: 多线程并行响应
```

二、关键优化点

1. ‌**动态线程池管理**‌
   - I/O线程数可运行时调整（默认4-16线程），通过`io-threads`参数配置
   - 智能负载均衡：根据连接数自动分配线程资源
2. ‌**网络处理优化**‌
   - 批量解析请求：减少线程切换开销，吞吐量较6.0提升40%
   - 响应缓冲区动态扩容：避免大包阻塞主线程
3. ‌**协议层增强**‌
   - 优先使用RESP3协议，支持多模态数据返回
   - 客户端缓存通知机制优化

------

三、与传统版本的差异对比

| 阶段           | Redis 6.0      | Redis 7.0改进         |
| -------------- | -------------- | --------------------- |
| ‌**网络I/O**‌    | 固定4线程处理  | 动态线程池+自适应缓冲 |
| ‌**命令执行**‌   | 严格单线程     | 保持单线程原子性      |
| ‌**大请求处理**‌ | 可能阻塞主线程 | 分片处理+零拷贝优化   |
| ‌**内存管理**‌   | 基础jemalloc   | 智能碎片整理算法      |

------





```mermaid
flowchart LR
    A[网络I/O] --> B{多线程实现}
    B -->|6.0版本| C[固定4线程]
    B -->|7.0版本| D[动态线程池]
    E[命令执行] --> F{单线程原子性}
    F -->|6.0| G[严格串行]
    F -->|7.0| H[保持兼容]
    I[内存管理] -->|6.0| J[基础jemalloc]
    I -->|7.0| K[智能碎片整理]

```



四、性能表现

- ‌**延迟**‌：P99延迟降低30%，尤其改善10KB以上大包场景5
- ‌**吞吐量**‌：单节点QPS可达15万+（16线程配置）35
- ‌**资源利用率**‌：CPU利用率提升至65%（原50%）17

> 注：需在redis.conf中配置`io-threads-do-reads yes`启用多线程读

### 2-4、使用及其常见问题

2-4-1、

2-4-2、

#### 2-4-3、缓存不一致问题



2-4-4、

### 2-5、分布式集群

#### 2-5-1、Redis集群核心概念

1. 集群定义与背景

Redis集群是Redis提供的分布式数据库方案，通过分片(sharding)实现数据共享，并提供复制和故障转移功能。随着互联网应用数据量爆发式增长，单机Redis在存储容量、读写性能和高可用性等方面逐渐难以满足需求，集群方案应运而生。

2. 集群核心功能

- ‌**数据分片**‌：将数据分散存储在多个节点上，突破单机存储容量限制
- ‌**高可用性**‌：通过节点复制和自动故障转移确保服务连续性
- ‌**读写分离**‌：从节点分担读请求，提升系统读性能
- ‌**水平扩展**‌：通过增加节点实现容量和性能的线性增长

#### 2-5-2、集群架构与原理

1. 节点组成

- ‌**主节点(Master)**‌：负责处理槽位数据读写
- ‌**从节点(Slave)**‌：复制主节点数据，提供读服务
- ‌**推荐配置**‌：至少3个主节点，每个主节点配1个从节点

2. 数据分片机制

```java
// 键到槽位的哈希计算
slot = CRC16(key) % 16384  // 共16384个槽位
```

- 每个主节点负责部分槽位（默认每个节点5461个槽位）
- 客户端直接路由到负责对应槽位的节点

3. 节点通信

- 使用Gossip协议进行元数据同步
- 默认通过6379端口进行TCP/IP通信
- 心跳检测：节点间定期PING/PONG

#### 2-5-3、集群搭建实践

1. 环境准备

```bash
# 下载Redis
wget https://download.redis.io/releases/redis-7.0.5.tar.gz
tar -xzf redis-7.0.5.tar.gz
cd redis-7.0.5

# 启动6个节点(7000-7005端口)
for port in {7000..7005}; do
  ./src/redis-server --port $port --cluster-enabled yes --cluster-config-file nodes-$port.conf &
done:ml-citation{ref="3" data="citationList"}
```

2. 集群创建

```bash
# 使用redis-cli创建集群
redis-cli --cluster create \
  127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 \
  127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 \
  --cluster-replicas 1:ml-citation{ref="3" data="citationList"}
```

3. 集群验证

```bash
redis-cli -p 7000 cluster nodes  # 查看节点列表
redis-cli -p 7000 cluster info  # 查看集群状态:ml-citation{ref="3" data="citationList"}
```

#### 2-5-4、集群核心机制

1. 故障转移

- 主节点故障时，从节点自动升级为主节点
- 故障检测通过节点间心跳实现
- 需多数节点确认故障才会触发转移

2. 数据迁移

- 使用`CLUSTER ADDSLOTS`手动分配槽位
- 支持在线重新分片(resharding)
- 迁移过程中保证数据一致性

3. 客户端路由

- 智能客户端(如JedisCluster)自动处理重定向
- MOVED错误：永久重定向到正确节点
- ASK错误：临时重定向用于迁移场景

#### 2-5-5、集群优化建议

1. 性能调优

- ‌**合理分片**‌：避免数据倾斜，均匀分布槽位
- ‌**连接池配置**‌：优化客户端连接参数
- ‌**批量操作**‌：使用Pipeline减少网络往返

2. 高可用保障

- 每个分片配置至少一个从节点
- 跨机房部署提升容灾能力
- 监控集群状态和性能指标

3. 扩容策略

- 预先规划容量，避免频繁扩容
- 扩容后执行重新分片平衡数据
- 使用`--cluster-use-empty-masters`避免空节点

#### 2-5-6、与其他方案对比

| ‌**方案**‌ | ‌**优点**‌                 | ‌**缺点**‌                       |
| -------- | ------------------------ | ------------------------------ |
| 主从复制 | 实现简单，读写分离       | 写能力单点瓶颈，无自动故障转移 |
| 哨兵模式 | 自动故障转移，高可用     | 仍然存在写单点问题             |
| 集群模式 | 真正分布式，支持水平扩展 | 配置复杂，客户端需支持         |

> 📌 注：Redis集群适用于需要处理海量数据和高并发的场景，对于中小规模应用，哨兵模式可能更简单实用

#### 

## 3、MySQL

​	关系型数据库，强结构型数据库，5.5版本后为默认存储引擎innoDB

### 3-1、MySQL基本结构

### 3-2、MySQL存储引擎

### 3-3、MySQL优化分析

# 二、Spring全家桶

## 1、SpringBoot

​	SpringBoot是Spring的成功后继者，继承了优秀的IOC/DI、AOP、模块化特点，IOC是设计原则，DI是实现

DI实现方式：

1、构造器注入（推荐，后面讲）

2、Setter注入

3、field注入

### 1-1、三大核心特性

#### 1-1-1、自动装配

​	1、开发者仅需引入依赖添加配置信息即可使用，是因为springboot是基于约定的，所有组件都需要基于starter引入到springboot框架，约定每一个自定义starter中都需要在META-IFO文件夹下创建spring.factories文件，此文件对应着自动配置类的完全限定名。

2、springboot容器会扫描该文件，涉及@autoconfigration@SelectorImport以及相关接口

#### 1-1-2、starter依赖

伟大的约定，具有了第三方的扩展能力。

自定义starter

一、引入starter相关依赖包（starter、process、parent）

二、编写自动配置类

三、编写ConfigrationProperties类，接收配置信息

四、（核心）classpath下创建META-IFO文件夹，在该文件夹下创建spring.factories。将自动配置类引入到这里。

#### 1-1-3、嵌入式服务器

tomcat、jetty、undertow

### 1-2、注解的使用

#### 1-2-1、常见注解

@Service、@Component、@Controller、@Configuration、@Repository

@Bean、@postConstruct

@Autowired、@Resource、@scope

@RequestMapping @Pathvariable  @RequestParam @RequestBody



#### 1-2-2、使用场景

行一、注册为spring容器的bean

行二、@bean可以灵活地注册为bean，使用第三方组件想进入spring容器的

行三、数据请求时，@RequestMapping用于地址uri映射，@Pathvariable用于url的？前面参数，@RequestParam用于？后面参数，@RequestBody用于请求体

### 1-3、理解spring容器

applicationcontext继承了beanFactory根接口，并且扩展了国际化、监听器注册等功能。



#### 1-3-1、beanFactory和FactoryBean区别？

1、beanFactory是spring容器的根接口，getBean、containsBean、getType方法。是spring容器的顶层设计

2、FactoryBean其实就是工具人，具备生产某一种bean的能力。getObject、getObjectType

**核心区别对比**‌

| ‌**维度**‌         | ‌**BeanFactory**‌             | ‌**FactoryBean**‌                            |
| ---------------- | --------------------------- | ------------------------------------------ |
| ‌**角色**‌         | 容器（管理所有 Bean）       | 特殊 Bean（生产其他 Bean）                 |
| ‌**获取对象**‌     | 直接返回 Bean 实例          | 返回 `getObject()` 方法创建的代理/复杂对象 |
| ‌**设计目的**‌     | 提供基础的依赖注入能力      | 扩展 Spring 的实例化逻辑                   |
| ‌**典型使用场景**‌ | 所有 Spring Bean 的基础管理 | 集成第三方框架（如 MyBatis、Hibernate）    |

#### 1-3-2、bean的生命周期

1、加载beanDefinition->实例化->属性填充->初始化->缓存使用->销毁

2、详细阶段说明

1. ‌**加载Bean定义**‌
   - 容器解析XML/注解配置生成BeanDefinition
   - 触发`BeanFactoryPostProcessor`修改定义
2. ‌**实例化**‌
   - 调用构造器创建原始对象（反射或工厂方法）
   - 注意：此时属性均为默认值
3. ‌**属性填充**‌
   - 通过`populateBean()`完成依赖注入
   - 包括@Autowired/@Value等注解处理
4. ‌**Aware接口注入**‌
   - 回调`BeanNameAware`/`ApplicationContextAware`等接口
5. ‌**初始化扩展点**‌
   - `@PostConstruct`标注的方法
   - `InitializingBean.afterPropertiesSet()`
   - XML中配置的`init-method`
6. ‌**AOP代理生成**‌
   - 通过`BeanPostProcessor.postProcessAfterInitialization`创建代理对象
7. ‌**销毁阶段**‌
   - `@PreDestroy`标注的方法
   - `DisposableBean.destroy()`
   - XML中配置的`destroy-method`

```mermaid
graph LR
    A[加载Bean定义] --> B[实例化]
    B --> C[属性填充/DI]
    C --> D[Aware接口注入]
    D --> E[BeanPostProcessor前置处理]
    E --> F[初始化方法]
    F --> G[BeanPostProcessor后置处理]
    G --> H[使用中]
    H --> I[销毁前回调]
    I --> J[销毁方法]

```

#### 1-3-3、循环依赖

问题描述

1. ‌**属性注入循环依赖**‌

   ```java
   @Component
   public class A {
       @Autowired
       private B b;  // A 依赖 B
   }
   
   @Component
   public class B {
       @Autowired
       private A a;  // B 又依赖 A
   }
   ```

   - ‌**解决方式**‌：通过 ‌**三级缓存**‌ 提前暴露未初始化的 Bean 引用。

2. ‌**构造器注入循环依赖**‌

   ```java
   @Component
   public class A {
       private final B b;
       public A(B b) { this.b = b; }  // 构造器依赖 B
   }
   
   @Component
   public class B {
       private final A a;
       public B(A a) { this.a = a; }  // 构造器依赖 A
   }
   ```

   - ‌**无法解决**‌：Spring 会直接抛出 `BeanCurrentlyInCreationException`

三级缓存定义与作用

1. ‌**一级缓存（singletonObjects）**‌
   - 存储已完成初始化（实例化+依赖注入+初始化）的完整Bean
   - 通过`getBean()`直接获取的最终对象来源
2. ‌**二级缓存（earlySingletonObjects）**‌
   - 存储已实例化但未完成初始化的"早期对象"
   - 解决多线程环境下重复创建代理对象的问题
3. ‌**三级缓存（singletonFactories）**‌
   - 存储生成对象的`ObjectFactory`工厂（Lambda表达式）
   - 核心作用：在存在AOP代理时，确保返回的是代理对象而非原始对象



```mermaid
graph LR
    A1[开始创建A] --> A2[实例化A对象]
    A2 --> A3[将A的ObjectFactory放入三级缓存]
    A3 --> A4[发现需注入B]
    A4 --> B1[开始创建B]
    B1 --> B2[实例化B对象]
    B2 --> B3[将B的ObjectFactory放入三级缓存]
    B3 --> B4[发现需注入A]
    B4 --> C1[从三级缓存获取A的ObjectFactory]
    C1 --> C2[执行工厂方法获得早期A对象]
    C2 --> B5[将早期A对象移入二级缓存]
    B5 --> B6[完成B的属性注入]
    B6 --> A5[继续完成A的属性注入]
    A5 --> A6[初始化完成后将A移入一级缓存]

```

### 1-4、事务管理

#### 1-4-1、声明式

@transaction

#### 1-4-2、编程式

transactiontemplate

#### 1-4-3、事务传播



## 2、SpringCloud升级

### 2-1、基本五大组件

注册中心：eureka/consul/Nacos/zookeeper

负载均衡：Ribbon

远程调用：feign

网关：gateway/zunnl

熔断：Hystrix

### 2-2、如何理解各组件

#### 2-2-1 注册中心

​	eureka是ap模型，注重可用性，zookeeper是cp，侧向于一致性结果

#### 2-2-2 负载均衡

​	Ribbon和feign组合使用，在调用过后，需要执行相应的负载均衡策略，选择其中一个实例进行转发。

IRule自定义负载均衡策略，常见有轮询/权重/随机/最好可行

#### 2-2-3 网关

​	对外接口的聚合，提供认证鉴权/动态路由/流量控制/请求响应处理;权限控制、负载均衡、路由转发、监控、安全控制黑名单和白名单等

认证鉴权：登陆验证（Qauth2/JWT）

动态路由：屏蔽内部接口（安全性），通过路由转发提供服务

流量控制：应对大流量时，能够提供防火墙功能，防止冲垮内部服务，配置独立的线程池/接口限流/服务降级等

请求响应再处理：通过前置/后置过滤器

- ‌**前置过滤器（Pre Filter）**‌

  - 在请求路由到下游服务前执行，可进行请求头修改、参数校验等操作
  - 典型应用场景：JWT 鉴权、请求参数加密、流量标记

- ‌**后置过滤器（Post Filter）**‌

  - 在下游服务返回响应后执行，用于修改响应数据或记录日志
  - 典型应用场景：响应数据脱敏、统一错误格式封装

  ### 过滤器分类

  | 类型           | 作用范围 | 执行阶段 | 示例                                     |
  | -------------- | -------- | -------- | ---------------------------------------- |
  | ‌**全局过滤器**‌ | 所有路由 | Pre/Post | `NettyWriteResponseFilter`（响应写入）25 |
  | ‌**局部过滤器**‌ | 指定路由 | Pre/Post | `AddRequestHeader`（添加请求头）         |

```java
@Component
@Order(-1)  // 最高优先级
public class AuthFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // Pre 处理逻辑
        return chain.filter(exchange).then(Mono.fromRunnable(() -> {
            // Post 处理逻辑
        }));
    }
}

```

```mermaid
graph LR
A[客户端请求] --> B{路由匹配}
B -->|匹配成功| C[Pre过滤器]
C --> D[下游服务]
D --> E[Post过滤器]
E --> F[返回响应]

```

#### 2-2-4 feign

​	Rest API风格的调用，定义接口，定义方法，通过动态代理实现调用。

#### 2-2-5 熔断降级

Hystrix和reillence4j。

熔断降级有什么区别？



### 2-3、SpringCloud alibaba中的Nacos

alibaba cloud中服务注册中心，同时是配置中心

#### 2-3-1 注册中心

配置服务器地址添加必要注解，根据server.name注册服务

#### 2-3-2 配置中心

使用方法：命名空间隔离+多层次配置

云环境中通常在部署实例时，会在镜像脚本Dockfile中添加 [CMD] jar -jar .... spring.profiles.active=dev/test/prod

参数值随着不同的环境变化，从而起到加载不同配置文件作用，通常配置文件数据私密，不会在服务中出现，只会配置不同隔离环境的命名空间，从Nacos的服务器中获取配置信息。

```yaml
--spring.profiles.active=dev、test、prod
可以在application.yaml文件中单独配置
# 测试环境独立配置
spring:
  profiles: test
  cloud:
    nacos:
      config:
        server-addr: 192.168.1.100:8848  # 测试环境Nacos地址
        namespace: test-ns-id

# 生产环境独立配置
---
spring:
  profiles: prod
  cloud:
    nacos:
      config:
        server-addr: 10.0.0.100:8848  # 生产环境Nacos地址
        namespace: prod-ns-id
进行环境隔离
```

#### 2-3-3 配置动态更新

核心原理

1.长轮询监听。客户端向服务端注册一个监听器，服务端开始数30s，30s内有变更，通知给客户端

```mermaid
sequenceDiagram
    客户端->>Nacos Server: 发起配置查询请求（携带MD5值）
    alt 配置有变更
        Nacos Server-->>客户端: 立即返回新配置
    else 无变更
        Nacos Server->>客户端: 保持连接30秒
        loop 监控变更
            Nacos Server-->>客户端: 配置变更时响应
        end
    end

```

2.事件驱动更新

有变更，通知给客户端，本地通过@RefreshScope或者@ConfigurationProperties重新初始化

#### 2-3-4 Nacos 中的实现细节

1. ‌**客户端行为**‌
   - 首次启动拉取配置后，立即注册监听器并开启长轮询
   - 每次请求携带本地配置的MD5值，服务端通过对比判断是否变更
2. ‌**服务端处理**‌
   - 无变更时：TCP连接保持打开状态（非HTTP轮询），减少重复握手开销
   - 有变更时：立即返回新配置并关闭当前连接
3. ‌**超时控制**‌
   - 默认30秒超时后，客户端会重新发起长轮询请求
   - 服务端配置`nacos.config.long-poll.timeout`可调整该阈值

# 三、计算机网络

## 1 

# 四、Java仔摸底

## 1、JVM虚拟机运行时数据区

## 2、基础知识点汇总

## 3、数据结构

### 4 、GC算法

# 五、消息队列

## 1、rabbitMQ介绍

轻量级消息队列系统

## 2、rocketMQ理解

百万级

## 3、kafaka应用

千万级

### 3-1 

### 3-2

### 3-3

## 4、MQTT协议

# 六、常用框架解析

## 1、Netty好，得学

异步事件驱动的高性能网络通信应用框架，用于快速开发高性能、高可靠性的网络服务器、客户端程序。运用在各个框架组件上Dubbo、gRPC、springgateway、xxl-job等等

### 1-1、基础汇总

#### 1-1-1、‌Netty核心概念‌

1. ‌**定义与定位**‌
   Netty是一个‌**异步事件驱动**‌的网络应用框架，用于快速开发高性能、高可靠性的网络服务器和客户端程序。
   - ‌**地位**‌：在Java网络框架中的地位类比Spring在JavaEE中的地位。
   - ‌**应用场景**‌：Cassandra、Spark、RocketMQ等知名框架均基于Netty实现网络通信。
2. ‌**核心优势**‌
   - ‌**高性能**‌：基于NIO（非阻塞IO），支持零拷贝机制优化数据传输；
   - ‌**易用性**‌：统一API（支持HTTP/Socket/WebSocket等），简化复杂网络编程；
   - ‌**扩展性**‌：模块化设计（如灵活的线程模型、可插拔的组件）。

------

#### 1-1-2、‌核心组件与架构‌

1. ‌**核心组件**‌

   | ‌**组件**‌          | ‌**作用**‌                                                     |
   | ----------------- | ------------------------------------------------------------ |
   | `EventLoop`       | 事件循环线程，处理I/O操作和任务调度（每个`Channel`绑定一个`EventLoop`）； |
   | `Channel`         | 网络连接抽象，支持TCP/UDP等协议；                            |
   | `ChannelPipeline` | 责任链模式，串联多个`ChannelHandler`处理数据（如编解码、业务逻辑）； |
   | `ByteBuf`         | 高性能字节缓冲区，支持池化、直接内存分配和动态扩容。         |

2. ‌**线程模型**‌

   - ‌**主从Reactor多线程**‌：改进自Reactor模式，`bossGroup`处理连接，`workerGroup`处理I/O；
   - ‌**对比传统NIO**‌：解决NIO的API复杂性和Epoll空轮询Bug。

------

#### 1-1-3、‌关键特性‌

1. ‌**零拷贝机制**‌
   通过`FileRegion`和`CompositeByteBuf`减少内存拷贝次数，提升大文件传输效率。
2. ‌**协议支持**‌
   - 内置HTTP/WebSocket/SSL等协议；
   - 支持Protobuf等序列化框架。
3. ‌**动态扩展能力**‌
   - ‌**拦截器**‌：通过`ChannelHandler`实现日志、加密等功能；
   - ‌**流量控制**‌：基于`ChannelFuture`的异步回调机制。

------

#### 1-1-4、‌Netty与NIO的关系‌

1. ‌**基于NIO优化**‌
   - 解决原生NIO的复杂性（如`Selector`、`Buffer`手动管理问题）；
   - 提供更友好的API（如`ByteBuf`替代`ByteBuffer`）。
2. ‌**性能对比**‌
   - ‌**吞吐量**‌：Netty的零拷贝和线程模型显著优于传统NIO；
   - ‌**稳定性**‌：避免NIO的Epoll空轮询导致的CPU 100%问题。

------

#### 1-1-5、‌典型应用场景‌

1. ‌**RPC框架**‌（如Dubbo）
   利用Netty实现高性能远程调用。
2. ‌**消息队列**‌（如RocketMQ）
   处理高并发网络通信。
3. ‌**实时通信**‌
   支持WebSocket长连接（如聊天室、推送系统）。

### 1-2、原理剖析



### 1-3、疑难杂症

#### 1-3-1、如何解决nio空轮训

​	一、‌**问题背景**‌

NIO 的 `Selector.select()` 方法在无事件时可能因底层 `epoll` 实现缺陷‌**提前返回**‌（返回 `0` 但未阻塞），导致 CPU 空转（100% 占用）。

- ‌**触发条件**‌：`Selector` 无就绪事件却‌**非预期唤醒**‌，且连续发生。
- ‌**影响**‌：`EventLoop` 线程空跑，浪费资源

二、‌**解决方案核心逻辑**‌

Netty 通过 ‌**检测-重建**‌ 机制规避该问题：

1. ‌**空轮询计数**‌
   - 每次 `select()` 调用后，`selectCnt` 计数器自增；
   - 若正常唤醒（有事件/超时），重置 `selectCnt=0`。
2. ‌**阈值判定**‌
   - 当 `selectCnt` 超过阈值（默认 `512`），判定为 NIO 空轮询 Bug。
3. ‌**Selector 重建**‌
   - ‌**创建新 Selector**‌：替换旧实例36；
   - ‌**迁移 Channel**‌：将旧 `Selector` 注册的 `Channel` 重新绑定到新 `Selector`；
   - ‌**关闭旧 Selector**‌：释放资源。

------

三、‌**关键实现细节**‌

1. ‌**检测逻辑**‌

   - 通过unexpectedSelectorWakeup()方法判断异常唤醒：

     ```java
     if (selectedKeys == 0 && !hasTasks() && selectCnt > threshold) {
         rebuildSelector(); // 触发重建
     }
     ```

   - 结合‌**历史平均事件数**‌辅助判断（避免误判）。

2. ‌**重建流程**‌

   - 反射获取旧 `Selector` 的 `selectedKeys` 和 `publicSelectedKeys` 字段；
   - 通过 `rebuildSelector0()` 完成迁移。

3. ‌**性能优化**‌

   - ‌**延迟重建**‌：仅当连续空轮询超阈值时触发，避免频繁开销；
   - ‌**线程安全**‌：迁移期间暂停事件处理，保证一致性

检测+重建

```mermaid
graph TD
    A[Selector.select方法] --> B{是否正常唤醒?}
    B -->|是| C[重置 selectCnt=0]
    B -->|否| D[selectCnt++]
    D --> E{selectCnt > 512?}
    E -->|是| F[重建Selector]
    E -->|否| A
    F --> G[迁移Channel]
    G --> H[关闭旧Selector]

```



#### 1-3-2、如何解决粘包拆包

一、‌**问题本质与原因**‌

1. ‌**粘包**‌：多个数据包被合并为一个TCP包发送，接收端无法区分原始消息边界。

2. ‌**拆包**‌：单个数据包被拆分为多个TCP包传输，接收端需重组。

3. ‌**根本原因**‌：TCP是面向字节流的协议，无消息边界，且受TCP缓冲区大小、Nagle算法等影响。

4. 一、‌**粘包现象（多个包合并）**‌

   1. ‌**案例1：连续小包合并**‌
      - ‌**场景**‌：客户端连续发送两条短消息`"HELLO"`和`"WORLD"`，服务端却一次性收到`"HELLOWORLD"`。
      - ‌**原因**‌：TCP缓冲区将多个小包合并发送（Nagle算法或缓冲区未满）。
   2. ‌**案例2：数据粘连**‌
      - ‌**场景**‌：A包`"ABC"`和B包`"123"`被合并为`"ABC123"`，无法区分原始消息边界。

   ------

   二、‌**拆包现象（单包拆分）**‌

   1. ‌**案例1：大数据包分割**‌
      - ‌**场景**‌：发送`"NETTY_IS_GOOD"`，接收端分两次收到`"NETTY_"`和`"IS_GOOD"`。
      - ‌**原因**‌：数据超过TCP缓冲区大小（MSS限制）。
   2. ‌**案例2：半包+粘包混合**‌
      - ‌**场景**‌：A包`"DATA"`和B包`"STREAM"`被拆分为`"DAT"`（A包部分）和`"ASTREAM"`（A包剩余+B包）。

5. 

   ```mermaid
   graph LR
       A[发送端] -->|正常包| B[包1+包2独立]
       A -->|粘包| C[包1+包2合并]
       A -->|拆包| D[包1拆分->包1_1+包1_2]
       A -->|混合| E[包1_1+包2粘合]
   
   ```

   

------

二、‌**核心解决方案**‌

Netty通过‌**编解码器（Encoder/Decoder）**‌实现以下四种主流方案：

1. ‌**固定长度协议（FixedLengthFrameDecoder）**‌

- ‌**原理**‌：强制每个数据包为固定长度（如1024字节），不足则补空字符。

- ‌**适用场景**‌：简单协议，数据长度稳定。

- ‌代码示例：

  ```java
  pipeline.addLast(new FixedLengthFrameDecoder(1024));
  ```

2. ‌**分隔符协议（DelimiterBasedFrameDecoder）**‌

- ‌**原理**‌：以特殊字符（如`\n`、`\r\n`）标记包结尾。

- ‌**优势**‌：灵活，适合文本协议（如HTTP）。

- ‌代码示例‌：

  ```java
  pipeline.addLast(new DelimiterBasedFrameDecoder(1024, Unpooled.wrappedBuffer("\n".getBytes())));
  ```

3. ‌**长度字段协议（LengthFieldBasedFrameDecoder）**‌

- ‌**原理**‌：自定义协议头包含长度字段，接收端按长度截取完整包。

- ‌结构：

  ```text
  +--------+----------+  
  | Length | Content  |  
  +--------+----------+  
  ```

- ‌代码示例：

  ```java
  pipeline.addLast(new LengthFieldBasedFrameDecoder(1024, 0, 4, 0, 4));
  ```

4. ‌**自定义协议（MessageToMessageCodec）**‌

- ‌**原理**‌：完全自定义编解码逻辑，如添加魔数、校验位等。
- ‌**适用场景**‌：复杂业务协议（如RPC）。
- ‌关键步骤‌：
  - 定义协议类（含长度字段`len`和内容`content`）；
  - 实现`encode()`和`decode()`方法。

------

三、‌**方案对比与选型**‌

| ‌**方案**‌   | ‌**优点**‌           | ‌**缺点**‌             | ‌**适用场景**‌         |
| ---------- | ------------------ | -------------------- | -------------------- |
| 固定长度   | 实现简单，解析高效 | 浪费带宽（补空字符） | 固定格式的二进制协议 |
| 分隔符     | 灵活，兼容文本协议 | 需转义分隔符，防冲突 | HTTP、日志流         |
| 长度字段   | 精准控制，扩展性强 | 协议设计稍复杂       | 高性能RPC/消息队列   |
| 自定义协议 | 完全适配业务需求   | 开发成本高           | 复杂私有协议         |

------

四、‌**底层机制与优化**‌

1. ‌**ByteBuf处理**‌：Netty的`ByteBuf`支持动态扩容和切片，避免拆包时的内存拷贝。
2. ‌**滑动窗口**‌：通过`ChannelPipeline`的链式处理逐步解析数据。
3. ‌**异常处理**‌：自动丢弃不完整包或触发`TooLongFrameException`

```mermaid
graph TD
    A[粘包/拆包问题] --> B{解决方案}
    B --> C[固定长度]
    B --> D[分隔符]
    B --> E[长度字段]
    B --> F[自定义协议]
    C --> G[FixedLengthFrameDecoder]
    D --> H[DelimiterBasedFrameDecoder]
    E --> I[LengthFieldBasedFrameDecoder]
    F --> J[MessageToMessageCodec]

```

#### 1-3-3、零拷贝技术

‌**定义**‌

- 零拷贝（Zero-Copy）通过减少或消除数据在内存间的冗余拷贝，直接在内核空间完成数据传输，显著提升I/O性能

```mermaid
graph TD
    subgraph 传统I/O[传统I/O:4次拷贝]
        A[磁盘] -->|DMA拷贝| B[内核缓冲区]
        B -->|CPU拷贝| C[用户缓冲区]
        C -->|CPU拷贝| D[Socket缓冲区]
        D -->|DMA拷贝| E[网络设备]
    end
    

    subgraph 零拷贝[零拷贝:2次拷贝]
        F[磁盘] -->|DMA拷贝| G[内核缓冲区]
        G -->|DMA拷贝| H[网络设备]
    end

```



## 2、Mybatis的使用

半ORM框架，灵活好用

### 2-1、使用方式之两种

#### 2-1-1、接口方法上注解

​	简单使用CRUD时，接口方法上添加@select@update@insert@delete，加上具体SQL即可操作数据库

```java
核心实现逻

在 Java 接口方法上直接使用 @Select、@Insert 等注解编写 SQL，无需 XML 文件。
示例代码：
java
@Insert("INSERT INTO user(name) VALUES(#{name})")
void insertUser(@Param("name") String name);
通过 SqlSession.getMapper() 获取代理对象调用方法。
‌适用场景‌

简单 CRUD 操作或快速原型开发；
微服务等轻量级项目，减少配置文件数量。
‌优势‌

代码简洁，开发效率高；
避免 XML 与 Java 代码频繁切换
```



#### 2-1-2、mapper.xml

这种适合复杂项目，接口和mapper文件一一对应，声明同一命名空间

```xml
核心实现逻

通过编写 mapper.xml 文件定义 SQL 语句，与 Java 接口方法绑定，实现 SQL 与代码分离。
典型配置示例：
<insert id="batchInsertData" parameterType="list">
    INSERT INTO table_name (字段) VALUES 
    <foreach item="item" collection="list" separator=",">
        (#{item.property})
    </foreach>
</insert>
支持动态 SQL 和批量操作。
‌适用场景‌
复杂 SQL 语句（如多表关联、动态条件）；
需要精细控制 SQL 性能优化的场景（如批量插入）。
‌优势‌
SQL 可视化，便于维护和调试；
支持高级映射（如结果集嵌套）
    #和$在SQL语句中使用，#是参数
```

#### 2-1-3、#和$在SQL语句中的使用区别

------

一、‌**核心机制差异**‌

1. ‌**`#{}`（预编译占位符）**‌

   - 原理：转换为 JDBC 的 `PreparedStatement` 参数占位符（`?`），执行时安全绑定参数值。

   - 示例：

     ```xml
     SELECT * FROM user WHERE id = #{id}
     ```

     解析为：

     ```xml
     SELECT * FROM user WHERE id = ?
     ```

   - ‌**安全性**‌：自动转义特殊字符，防止 SQL 注入。

2. ‌**`${}`（字符串直接替换）**‌

   - 原理：直接拼接参数值到 SQL 中，无预编译处理。

   - 示例：

     ```xml
     SELECT * FROM ${tableName} WHERE id = 1
     ```

     若tableName=user，解析为：

     ```xml
     SELECT * FROM user WHERE id = 1
     ```

   - ‌**风险**‌：存在 SQL 注入漏洞（如输入 `' OR 1=1 --` 可篡改逻辑）。

------

二、‌**使用场景对比**‌

| ‌**场景**‌                   | ‌**推荐方式**‌ | ‌**原因**‌                                                 |
| -------------------------- | ------------ | -------------------------------------------------------- |
| 普通参数绑定（如ID、数值） | `#{}`        | 安全、高效（预编译复用）                                 |
| 动态表名/列名              | `${}`        | 需直接拼接 SQL 片段（如分表场景）                        |
| 排序字段（`ORDER BY`）     | `${}`        | `#{}` 会添加引号导致语法错误（如 `ORDER BY "age"` 无效） |

------

三、‌**性能与类型处理**‌

1. ‌**`#{}` 优势**‌
   - ‌**预编译缓存**‌：同一 SQL 多次执行仅编译一次，提升效率；
   - ‌**自动类型转换**‌：Java 类型与 JDBC 类型自动匹配（如 `String` → `VARCHAR`）。
2. ‌**`${}` 限制**‌
   - ‌**无类型处理**‌：直接拼接可能导致类型错误（如字符串未加引号）；
   - ‌**动态分表需配置**‌：需设置 `statementType="STATEMENT"` 禁用预编译。

------

四、‌**安全建议**‌

1. 优先使用 `#{}`，仅在必须动态拼接 SQL 时使用 `${}`；
2. 使用 `${}` 时，必须对输入值做严格校验或白名单过滤。

------

总结流程图

```mermaid
graph LR
    A[参数占位符] --> B[#]
    A --> C[$]
    B --> D[预编译防注入]
    B --> E[自动类型转换]
    C --> F[动态SQL拼接]
    C --> G[需手动防注入]
```

### 2-2、常用标签

#### 2-2-1、基础CRUD

1. ‌**`<select>`**‌

   - ‌**作用**‌：执行查询操作，将结果映射到 Java 对象。

   - ‌关键属性：

     - `id`：唯一标识符，对应接口方法名；
     - `resultType`/`resultMap`：指定返回类型或自定义映射规则；
     - `parameterType`：参数类型（可省略，自动推断）。

   - ‌示例：

     ```xml
     <select id="findUserById" resultType="User">
         SELECT * FROM user WHERE id = #{id}
     </select>
     ```

2. ‌**`<insert>`**‌

   - ‌**作用**‌：插入数据，支持主键回填。

   - ‌关键属性：

     - `useGeneratedKeys`：启用自增主键；
     - `keyProperty`：主键值赋给对象的哪个属性。

   - ‌示例：

     ```xml
     <insert id="addUser" parameterType="User" useGeneratedKeys="true" keyProperty="id">
         INSERT INTO user (name) VALUES (#{name})
     </insert>
     ```

3. ‌**`<update>` 与 `<delete>`**‌

   - ‌**共同点**‌：均需通过 `id` 绑定方法，`parameterType` 指定参数。
   - ‌**注意**‌：必须包含条件（如 `WHERE`），否则会操作全表

#### 2-2-2、动态标签

1. ‌**条件控制标签**‌

   - ‌`<if>`：根据条件动态拼接 SQL。

     ```xml
     <select id="findUser">
         SELECT * FROM user WHERE 1=1
         <if test="name != null">AND name = #{name}</if>
     </select>
     ```

   - ‌**`<choose>/<when>/<otherwise>`**‌：实现多条件分支（类似 `if-else`）。

2. ‌**循环与集合处理**‌

   - ‌`<foreach>`‌：遍历集合，常用于IN查询或批量操作。

     ```xml
     <delete id="batchDelete">
         DELETE FROM user WHERE id IN
         <foreach item="id" collection="list" open="(" separator="," close=")">
             #{id}
         </foreach>
     </delete>
     ```

3. ‌**辅助标签**‌

   - ‌**`<where>`**‌：智能添加 `WHERE` 关键字，自动去除多余 `AND`/`OR`；
   - ‌**`<set>`**‌：用于 `UPDATE` 语句，自动处理逗号。

------

#### 2-2-3、‌高级映射与复用标签‌

1. ‌**`<resultMap>`**‌

   - ‌**作用**‌：自定义复杂结果集映射（如一对一、一对多）。

   - ‌示例‌：

     ```xml
     <resultMap id="userMap" type="User">
         <id property="id" column="user_id"/>
         <result property="name" column="user_name"/>
     </resultMap>
     ```

2. ‌**SQL 片段复用**‌

   - ‌`<sql>` + `<include>`：定义可重用的 SQL 片段。

     ```xml
     <sql id="userColumns">id, name, age</sql>
     <select id="selectUser" resultType="User">
         SELECT <include refid="userColumns"/> FROM user
     </select>
     ```

------

#### 2-2-4、‌其他实用标签‌

- ‌**`<bind>`**‌：创建变量，简化表达式；
- ‌**`<trim>`**‌：灵活处理字符串前缀/后缀；

### 2-3、原理剖析

#### 2-3-1、整合springboot之自动配置

​	springboot有starter，mybatis通过自定义starter，整合到springboot框架中，基本流程就是META_INF下创建spring.factories文件，在此文件下配置autoconfigration的自定义配置类的完全限定名，springboot会自动扫描META_INF下的spring.factories文件中的自动配置类，从而自动配置和导入mybatis功能，开发者只需在配置文件中配置参数即可。

#### 2-3-2、MyBatis 运行原理深度解析

------

一、‌**核心架构分层**‌

MyBatis 采用分层设计，主要分为 ‌**接口层**‌、‌**核心处理层**‌ 和 ‌**基础支撑层**‌：

1. ‌接口层
   - 提供 `SqlSession` API（如 `selectOne()`、`insert()`），开发者通过它与 MyBatis 交互。
2. ‌核心处理层
   - ‌**SQL 解析**‌：将 XML注解中的 SQL 转换为 `MappedStatement` 对象；
   - ‌**参数映射**‌：将 Java 对象属性映射到 SQL 参数（`#{}` 替换为 `?`）；
   - ‌**SQL 执行**‌：通过 `Executor` 执行 SQL 并处理结果集。
3. ‌基础支撑层
   - 连接管理（`DataSource`）、事务控制（`Transaction`）、缓存（`Cache`）等。

------

二、‌**启动阶段：配置加载**‌

1. ‌解析配置文件
   - `SqlSessionFactoryBuilder` 读取 `mybatis-config.xml`，构建 `Configuration` 对象（全局配置中心）；
   - 加载 Mapper 接口和 XML 文件，解析为 `MappedStatement`（存储 SQL 和映射规则）。
2. ‌Mapper 动态代理
   - 通过 `MapperProxy` 为接口生成代理类，调用方法时关联对应的 `MappedStatement`。

------

三、‌**执行阶段：SQL 处理流程**‌

1. ‌SQL 会话创建

   - 通过 `SqlSessionFactory` 创建 `SqlSession`（封装了与数据库的交互）。

2. ‌参数处理

   - 将方法参数转换为 SQL 参数：

     ```java
     // 示例：方法调用
     userMapper.selectUserById(1);
     // 转换为 SQL：SELECT * FROM user WHERE id = ?
     ```

   - `TypeHandler` 处理 Java 类型与 JDBC 类型的转换。

3. ‌SQL 执行

   - ‌执行器（Executor）‌：
     - ‌**SimpleExecutor**‌：默认执行器，每次执行新 SQL；
     - ‌**ReuseExecutor**‌：复用预处理语句（`PreparedStatement`）；
     - ‌**BatchExecutor**‌：批量操作优化。
   - ‌缓存机制：
     - 一级缓存（`SqlSession` 级别）默认开启；
     - 二级缓存（`Mapper` 级别）需手动配置。

4. ‌结果映射

   - 通过 `ResultSetHandler` 将 `ResultSet` 转换为 Java 对象（依赖 `<resultMap>` 规则）。

------

四、‌**动态 SQL 处理**‌

1. ‌解析过程‌

   - XML 中的 `<if>`、`<foreach>` 等标签被解析为 `SqlNode` 对象；
   - 执行时根据参数动态拼接为完整 SQL（通过 `DynamicSqlSource`）。

2. ‌示例转换

   ```xml
   <select id="findUser">
       SELECT * FROM user 
       <where>
           <if test="name != null">AND name = #{name}</if>
       </where>
   </select>
   ```

   - 若参数 `name` 为 `null`，最终 SQL 为 `SELECT * FROM user`。

------

五、‌**扩展机制**‌

1. ‌插件（Interceptor）
   - 可拦截 `Executor`、`StatementHandler` 等组件，实现分页、性能监控。
2. ‌类型处理器（TypeHandler）
   - 自定义复杂类型（如 JSON、枚举）与数据库类型的转换。

------

总结：MyBatis 运行流程图

```mermaid
graph TD
    A[启动阶段] --> B[加载 mybatis-config.xml]
    B --> C[构建 Configuration 对象]
    C --> D[解析 Mapper.xml 为 MappedStatement]
    D --> E[生成 Mapper 接口代理类]
    
    F[执行阶段] --> G[调用 SqlSession API]
    G --> H[参数映射与 SQL 拼接]
    H --> I[Executor 执行 SQL]
    I --> J[结果集映射为对象]
    J --> K[返回结果]
```

‌**关键设计思想**‌：

- ‌**解耦 SQL 与代码**‌：通过 XML/注解集中管理 SQL；
- ‌**灵活映射**‌：`ResultMap` 处理复杂对象关系；
- ‌**动态扩展**‌：插件机制支持功能定制

# 七、常见业务系统中的核心

大型业务系统中核心接口请求量最大可达百万级，在如此巨大流量情况下如何有效维护系统稳定性至关重要。

## 1、常规业务系统介绍

```mermaid
graph TD
    %% 客户端层
    A[客户端] -->|HTTP/HTTPS| B(API网关)
    A -->|WebSocket| B
    A -->|移动端API| B

    %% API网关层
    B --> C[鉴权/限流]
    B --> D[动态路由]
    B --> E[熔断降级]
    C -->|JWT/OAuth2| F[认证服务]
    D --> G[服务发现]

    %% 业务服务层
    subgraph 微服务集群
        F --> H[用户服务]
        G --> I[商品服务]
        G --> J[订单服务]
        K[支付服务] --> L[消息队列]
    end

    %% 基础设施层
    H --> M[(MySQL主从)]
    I --> N[(Redis集群)]
    J --> O[(MongoDB分片)]
    L --> P[Kafka]
    P --> Q[日志服务]
    P --> R[库存服务]

    %% 容灾设计
    M --> S[数据库备份]
    N --> T[缓存预热]
    O --> U[数据分片迁移]

```

网约车项目

```mermaid
graph TD
    %% 用户端交互入口
    A[用户端] -->|1. 发起出行请求| B(订单服务)
    
    %% 订单服务协同流程
    B -->|2. 查询可用司机| C{司机状态服务}
    B -->|3. 请求路线规划| D[地图服务]
    B -->|4. 验证用户资质| E[用户管理服务]
    
    %% 司机状态服务联动
    C -->|5. 推送司机位置数据| B
    C -->|6. 同步接单状态变更| B
    C -->|7. 更新服务分数据| E
    
    %% 地图服务深度调用
    D -->|8. 获取实时交通数据| F[实时路况服务]
    D -->|9. POI数据检索| G[地理信息数据库]
    
    %% 订单完成流程
    B -->|10. 发起支付请求| H[支付服务]
    H -->|11. 返回支付结果| B
    B -->|12. 更新行程记录| I[日志服务]
    
    %% 异常处理链路
    C -->|13. 上报异常行为| J[风控服务]
    J -->|14. 触发司机限制| C

```



### 1-1、关键设计说明

1. ‌**分层架构**‌
   - ‌**客户端层**‌：支持多协议接入，适配Web/移动端/物联网设备
   - ‌**API网关层**‌：集成鉴权、动态路由和熔断功能，使用Spring Cloud Gateway或Zuul实现
   - ‌**服务集群**‌：按业务拆分独立服务，通过服务发现（如Nacos）动态注册节点
2. ‌**高并发设计**‌
   - 读写分离：MySQL主从库分离读写请求，Redis集群缓存热点数据
   - 异步处理：支付等耗时操作通过Kafka消息队列解耦，保障最终一致性
3. ‌**高可用机制**‌
   - 熔断降级：Hystrix/Sentinel实现服务级故障隔离
   - 多级缓存：Redis集群+本地缓存（Caffeine）减少数据库压力
   - 数据持久化：MongoDB分片存储非结构化数据，定期备份关键数据库
4. ‌**监控体系**‌（未图示）
   - Prometheus采集指标，Grafana可视化展示，ELK集中管理日志

## 2、常见业务系统中的问题



## 3、常用解决方案

## 5、如何系统性解决

​	云原生环境下，高并发高可用已为常态，





# 八、高级运维之云原生

## 1、自动化流水线

### 1-1、基础设施准备

1. ‌**代码托管**‌
   - 使用GitLab/Gitee管理微服务代码库，需配置SSH-KEY实现Jenkins无密拉取代码45
   - 分支策略建议：`main`-生产环境、`dev`-集成测试、`feature`-功能开发5
2. ‌**环境隔离**‌
   - 通过Kubernetes Namespace隔离开发/测试/生产环境8
   - 数据库按环境独立部署，使用Flyway管理Schema变更6

------

### 1-2、核心流水线设计

```mermaid
graph LR
    A[代码提交] --> B(Jenkins/GitLab CI触发)
    B --> C{并行构建}
    C --> D[单元测试]
    C --> E[代码扫描]
    D --> F[打包Docker镜像]
    E --> F
    F --> G[推送镜像仓库]
    G --> H[K8S滚动更新]
    H --> I[自动化测试]
    I --> J[生产发布]
```

1. ‌**构建阶段**‌
   - Maven多模块构建：按需单独编译特定服务（如网关）提升效率
   - 制品管理：将JAR包和Docker镜像存储到Nexus/Harbor
2. ‌**部署阶段**‌
   - 通过`kubectl apply -f deployment.yaml`更新K8S服务
   - Rancher流水线集成：自动执行镜像构建→仓库推送→集群部署
3. ‌**验证阶段**‌
   - 集成Postman自动化测试，结合Prometheus监控服务健康度

------

### 1-3、关键技术选型

| 组件          | 推荐方案                   | 作用                 |
| ------------- | -------------------------- | -------------------- |
| ‌**CI/CD引擎**‌ | Jenkins Pipeline/GitLab CI | 流水线编排与任务调度 |
| ‌**容器化**‌    | Docker + BuildKit          | 构建轻量化镜像       |
| ‌**编排工具**‌  | Kubernetes + Helm          | 服务部署与版本管理   |
| ‌**配置中心**‌  | Nacos/Apollo               | 多环境配置统一管理   |

------

### 1-4、高级优化项

1. ‌**安全加固**‌
   - 镜像扫描：Trivy集成到流水线检测漏洞
   - 密钥管理：Vault动态注入数据库密码
2. ‌**故障自愈**‌
   - 配置K8S Liveness探针自动重启异常容器
   - 日志告警：ELK收集错误日志触发企业微信通知
3. ‌**多云支持**‌
   - Terraform跨云编排基础设施，实现阿里云/华为云一键部署

------

### 1-5、典型问题解决方案

- ‌**OOM异常**‌：在K8S部署文件中限制容器内存，并配置`-XX:MaxRAMPercentage`参数
- ‌**构建缓慢**‌：使用Maven镜像仓库缓存依赖，并行构建独立微服务

> 注：完整实现参考阿里云效流水线或Rancher自动化部署模板





## 2、基础设施即服务

## 3、多租户共享资源



# 九、经典算法

## 1、排序算法

```mermaid
graph TD
A[排序算法]
B[比较类排序]
C[非比较类排序]
D[交换排序]
E[插入排序]
F[选择排序]
G[归并排序]
H[基数排序]
I[桶排序]
J[计数排序]
K[冒泡排序]
L[快速排序]
M[直接插入排序]
N[希尔排序]
O[简单选择排序]
P[堆排序]

A --> B
A --> C
B --> D
B --> E
B --> F
B --> G
C --> H
C --> I
C --> J
D --> K
D --> L
E --> M
E --> N
F --> O
F --> P

```



### 1-1、交换类

#### 1-1-1、冒泡排序

```java
fff
```

#### 1-1-2 快排

```java
public calss Main{
		public static void main(){
      int [] arr = {1,4,7,3,9,4,7,5,0};
      qucitSort(arr,0,arr.length-1);
    }
  public static void quickSort(int[] arr,int left,int right){
    
  }
  public static int partition(int [] arr,int left,int right){
    
  }
}
```



### 1-2、插入类

### 1-3、选择类

## 2、搜索算法



### 2-1、

## 3、

# 十、大数据时代

## 1 

## 2 常用大数据采集处理一体化



```mermaid
%% 大数据架构图（Mermaid 绘制）
flowchart TD
    %% ========== 数据采集层 ==========
    subgraph 数据采集层
        A[移动App/Web] -->|HTTP/日志| B[Flume/Nginx]
        C[IoT设备] -->|MQTT/传感器数据| D[Kafka Producer]
        E[数据库] -->|CDC日志| F[Debezium]
        B & D & F --> G[Kafka Cluster]
    end

    %% ========== 数据处理层 ==========
    subgraph 计算层
        G -->|实时流| H[Flink Streaming]
        G -->|批量数据| I[Flink Batch]
        H --> J[窗口聚合/CEP]
        I --> K[ETL/Join]
    end

    %% ========== 数据存储层 ==========
    subgraph 存储层
        J & K --> L[(HDFS/对象存储)]
        L --> M[Delta Lake/Iceberg]
        M --> N[数据湖元数据]
        L --> O[HBase/ClickHouse]
    end

    %% ========== 数据服务层 ==========
    subgraph 应用层
        O & M --> P[API服务]
        P --> Q[BI工具\nTableau/Superset]
        P --> R[实时大屏]
        P --> S[推荐系统]
    end

    %% ========== 资源管理层 ==========
    subgraph 资源管理层
        T[YARN/K8s] -->|资源调度| H & I
        U[Prometheus] -->|监控| G & H & T
    end

```



### **架构关键组件说明**‌

1. ‌**数据采集层**‌
   - ‌**Kafka**‌ 作为统一入口，接收来自 Flume（日志）、MQTT（IoT）、Debezium（数据库变更）的数据
   - 支持 ‌**流式**‌（实时）和 ‌**批量**‌（离线）两种摄入模式
2. ‌**计算层**‌
   - ‌**Flink Streaming**‌：处理实时数据（如风控、监控告警）
   - ‌**Flink Batch**‌：离线 ETL、数据清洗
   - 典型操作：窗口聚合、复杂事件处理（CEP）、多流 Join
3. ‌**存储层**‌
   - ‌**数据湖**‌（Delta Lake/Iceberg）：存储原始数据 + 结构化表
   - ‌**OLAP引擎**‌（ClickHouse/HBase）：支持高性能查询
   - ‌**元数据管理**‌：统一管理表结构、分区、版本
4. ‌**应用层**‌
   - 通过 ‌**REST API**‌ 对外提供数据服务
   - 支撑 BI 分析、实时大屏、推荐系统等场景
5. ‌**资源管理层**‌
   - ‌**YARN/K8s**‌：统一调度 Flink/Spark 计算资源
   - ‌**Prometheus**‌：监控 Kafka/Flink 的吞吐量、延迟等指标

------

### ‌**技术栈扩展建议**‌

- ‌**实时数仓**‌：用 Kafka + Flink + Hudi 构建 Lambda 架构
- ‌**数据治理**‌：集成 Apache Atlas 管理数据血缘
- ‌**机器学习**‌：在 Flink 中嵌入 TensorFlow/PyTorch 模型



## 3 Kafka 集群拓扑详细架构图

```mermaid
%% Kafka集群拓扑架构（Mermaid绘制）
flowchart TD
    %% ========== 生产者层 ==========
    subgraph 生产者层
        A[Web服务] -->|异步写入| B[Kafka Producer]
        C[IoT设备] -->|MQTT网关| B
        D[数据库CDC] -->|Debezium| B
    end

    %% ========== Kafka集群核心 ==========
    subgraph Kafka集群
        subgraph Broker1
            E[Leader Partition P0] -->|副本同步| F[Follower Partition P0]
            G[Leader Partition P1] -->|副本同步| H[Follower Partition P1]
        end

        subgraph Broker2
            I[Leader Partition P1] -->|副本同步| J[Follower Partition P1]
            K[Leader Partition P2] -->|副本同步| L[Follower Partition P2]
        end

        subgraph Broker3
            M[Leader Partition P2] -->|副本同步| N[Follower Partition P0]
            O[Leader Partition P0] -->|副本同步| P[Follower Partition P2]
        end
    end

    %% ========== 消费者层 ==========
    subgraph 消费者组1
        Q[Flink Worker1] -->|消费P0| E
        R[Flink Worker2] -->|消费P1| G
    end

    subgraph 消费者组2
        S[Spark Worker] -->|消费P2| K
    end

    %% ========== 协调服务 ==========
    Z[ZooKeeper集群] -->|选举Controller| Broker1
    Z -->|存储元数据| Broker2
    Z -->|监控状态| Broker3

    %% ========== 数据流向说明 ==========
    B -->|发布消息| Broker1 & Broker2 & Broker3
    style B stroke:#ff0000,stroke-width:2px
    style Q stroke:#0000ff,stroke-width:2px
    style S stroke:#0000ff,stroke-width:2px

```

### **核心组件说明**‌

1. ‌**Broker 节点**‌
   - 每个 Broker 包含多个 ‌**Partition（分区）**‌，分区分为 Leader（处理读写）和 Follower（副本）
   - 示例中展示 3 个 Broker 的副本分布（通过 `replication-factor=3` 实现高可用）
2. ‌**生产者/消费者**‌
   - ‌**生产者**‌：通过 `acks=all` 参数确保消息写入所有副本
   - ‌**消费者组**‌：不同组可重复消费，同组内分区分配给不同 Worker 实现并行
3. ‌**ZooKeeper 作用**‌
   - 管理 Broker 注册、Leader 选举（Kafka 3.0+ 可逐步替换为 KRaft 模式）
   - 存储 Topic/Partition 元数据（如 `__consumer_offsets`）
4. ‌**副本同步机制**‌
   - 绿色箭头表示 ISR（In-Sync Replicas）同步复制
   - 故障时自动切换 Leader（如 Broker1 宕机，P0 的 Follower 晋升为 Leader）

------

### ‌**关键配置参数**‌

| 参数                             | 说明                      | 推荐值              |
| -------------------------------- | ------------------------- | ------------------- |
| `num.partitions`                 | 每个 Topic 的分区数       | ≥消费者并行度       |
| `default.replication.factor`     | 副本数                    | 3（生产环境）       |
| `min.insync.replicas`            | 最小同步副本数            | 2（保证可用性）     |
| `unclean.leader.election.enable` | 允许不同步副本成为 Leader | false（防数据丢失） |

------

### ‌**扩展场景**‌

1. ‌跨机房部署

   ```mermaid
   flowchart LR
       A[生产者] -->|就近写入| B[本地Kafka集群]
       B -->|MirrorMaker2| C[异地灾备集群]
   ```

2. ‌Flink 精确一次消费

   ```java
   // 启用Checkpoint并配置Kafka偏移量提交
   env.enableCheckpointing(5000);
   props.setProperty("isolation.level", "read_committed");
   ```

## 4 Flink 作业链

以下是关于 ‌**Flink 作业链（Operator Chain）**‌ 的详细解析，综合了执行原理、优化机制及配置方法：

------

### ‌**1. 作业链的核心概念**‌

- ‌**定义**‌：Flink 会将多个算子（Operator）链接（Chain）成一个 ‌**Task Pipeline**‌，在同一个线程中执行，减少线程切换和网络序列化开销。
- ‌条件：
  - 算子间为 ‌**One-to-One**‌ 数据传输（如 `map`→`filter`）。
  - 并行度相同且未禁用链化。
- ‌**优化目标**‌：提升吞吐量，降低延迟。

------

### ‌**2. 作业链的生成流程**‌

Flink 执行图的转换过程中，作业链在 ‌**StreamGraph → JobGraph**‌ 阶段生成：

1. ‌**StreamGraph**‌：用户代码初始生成的逻辑拓扑，每个算子独立。
2. ‌JobGraph‌：
   - 合并符合条件的算子为 ‌**Operator Chain**‌（如 `Source`→`Map`→`Filter`）。
   - 每个 Chain 对应一个 ‌**Task**‌，由同一线程执行。

```mermaid
%% Flink作业链生成示例（Mermaid绘制）
flowchart LR
    A[Source] --> B[Map] --> C[Filter] --> D[Sink]
    style A fill:#f9f,stroke:#333
    style B fill:#bbf,stroke:#333
    style C fill:#bbf,stroke:#333
    style D fill:#f9f,stroke:#333
```

（虚线框内为合并后的 Task6）

------

### ‌**3. 关键配置与调优**‌

| ‌**配置项**‌                   | ‌**作用**‌                             | ‌**示例/建议**‌                |
| ---------------------------- | ------------------------------------ | ---------------------------- |
| `pipeline.operator-chaining` | 全局启用/禁用链化                    | `true`（默认）               |
| `disableOperatorChaining()`  | 代码中禁用特定算子的链化             | `map(...).disableChaining()` |
| `slotSharingGroup`           | 设置槽共享组，影响链内算子的资源分配 | `slotSharingGroup("groupA")` |

------

### ‌**4. 与资源调度的关系**‌

- ‌**Slot 共享**‌：同一作业链的算子默认共享 ‌**Slot 共享组**‌，避免跨节点通信。
- ‌**资源隔离**‌：通过 `slotSharingGroup` 强制拆分链，实现 CPU/内存隔离。

------

### ‌5. 常见问题‌

- ‌**性能下降**‌：链过长可能导致线程阻塞，需通过 `disableChaining` 拆分。
- ‌**调试工具**‌：通过 Flink WebUI 查看 ‌**Operator Chain**‌ 可视化效果

## 5 HDFS存储原始数据 → HBase提供实时查询接口‌？

以下是 ‌**HDFS 数据导入 HBase 实现实时查询**‌ 的完整方案，涵盖关键步骤、技术实现及优化建议：

------

### ‌**1. 数据准备与格式转换**‌

- ‌**HDFS 数据要求**‌
  确保原始数据包含 ‌**RowKey 设计**‌（如用户ID、时间戳等唯一标识），这是 HBase 高效查询的基础。

  - 若数据无明确 RowKey，需通过预处理生成（如 MD5 哈希或复合键）。

- ‌**格式转换工具**‌

  - ‌HBase BulkLoad：将 HDFS 数据转换为 HFile 格式（HBase 底层存储格式），避免直接写入的性能开销。

    ```bash
    # 示例：使用 MapReduce 生成 HFile
    hadoop jar hbase-mapreduce.jar importtsv \
      -Dimporttsv.columns=HBASE_ROW_KEY,cf:col1,cf:col2 \
      -Dimporttsv.bulk.output=/hfile_output_path \
      my_table /hdfs/input_path
    ```

------

### ‌**2. 数据导入 HBase**‌

#### ‌**方法一：BulkLoad（高吞吐）**‌

1. ‌**生成 HFile**‌：通过 `ImportTsv` 或自定义 MapReduce 作业转换数据。

2. ‌加载至 HBase‌：

   ```bash
   hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /hfile_output_path my_table
   ```

   优势：绕过 RegionServer 直接写 HDFS，适合海量数据初始化导入。

#### ‌**方法二：API 写入（实时增量）**‌

- 通过 ‌HBase Java API

  ‌ 逐行写入：

  ```java
  // 示例：Put 操作写入单行数据
  Table table = connection.getTable(TableName.valueOf("my_table"));
  Put put = new Put(Bytes.toBytes("row1"));
  put.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("col1"), Bytes.toBytes("value1"));
  table.put(put);
  ```

  适用场景：小规模实时数据同步。

------

### ‌**3. 实时查询优化**‌

- ‌**RowKey 设计**‌

  - ‌**散列化**‌：避免热点问题（如时间戳前缀加盐）。
  - ‌**范围查询优化**‌：按查询频率排序（如用户ID+时间戳）。

- ‌**索引与缓存**‌

  - ‌**Bloom Filter**‌：加速 RowKey 存在性判断，减少磁盘 IO。
  - ‌**Block Cache**‌：缓存频繁访问的数据块。

- ‌**预分区（Pre-splitting）**‌
  建表时按 RowKey 范围预分区，避免后期自动分裂导致的性能波动：

  ```
  create 'my_table', {NAME => 'cf'}, {SPLITS => ['a', 'm', 'z']}
  ```

------

### ‌**4. 验证与监控**‌

- ‌数据一致性检查

  通过 HBase Shell 或 API 抽样查询，对比 HDFS 原始数据：

  ```
  scan 'my_table', {LIMIT => 10}
  ```

- ‌性能监控

  - ‌**HBase Metrics**‌：关注 `RegionServer` 的读写延迟及 MemStore 使用情况。
  - ‌**HDFS 吞吐量**‌：监控 BulkLoad 期间的 HDFS 带宽占用。

------

### ‌**5. 典型问题解决**‌

- ‌**热点问题**‌：通过 ‌**Salting**‌ 或 ‌**Hash RowKey**‌ 分散写入压力。
- ‌**BulkLoad 失败**‌：检查 HFile 权限及 RegionServer 状态。

------

### ‌**总结流程**‌

```mermaid
graph LR
A[HDFS 原始数据] --> B{RowKey 设计}
B --> C[BulkLoad 生成 HFile]
B --> D[API 实时写入]
C --> E[加载至 HBase]
D --> E
E --> F[实时查询优化]
F --> G[监控与调优]
```

通过上述步骤，可实现 ‌**HDFS 冷数据**‌与 ‌**HBase 热查询**‌的高效协同。






## 附上

### Memaid语法解析

1. **`graph LR`**
   - 声明这是一个从左到右（Left-to-Right）的流程图
2. **节点定义**
   - `A[服务]`：矩形节点，显示文本为"服务"
   - `B(Fluentd/Filebeat)`：圆角矩形节点
   - `C[...]` 和 `D[...]`：同类型节点
3. **连接线**
   - `-->`：箭头连接线
   - `|日志输出|`：连线上的文字标签

### 支持的图标

|      |
| ---- |

| 类型   | 声明方式            | 用途               |
| :----- | :------------------ | :----------------- |
| 流程图 | `graph TD`/`LR` | 系统架构、流程描述 |
| 序列图 | `sequenceDiagram` | 交互时序           |
| 类图   | `classDiagram`    | 面向对象结构       |
| 状态图 | `stateDiagram`    | 状态转换           |
| 甘特图 | `gantt`           | 项目时间规划       |
| 饼图   | `pie`             | 比例分布           |
