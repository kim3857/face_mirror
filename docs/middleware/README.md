# 中间件模块导航

本模块聚焦常用中间件的原理、使用与优化，包括消息队列、缓存、搜索引擎等。通过本模块的学习，你将掌握企业级中间件的核心概念、部署方案和最佳实践。

## 🚀 快速开始

### 环境要求

- JDK 1.8+
- Maven 3.6+
- Docker (可选，用于容器化部署)

### 项目结构

```
middleware/
├── mq/                 # 消息队列相关实现
│   ├── rabbitmq/      # RabbitMQ 实现
│   └── kafka/         # Kafka 实现
├── cache/             # 缓存中间件实现
│   └── redis/         # Redis 实现
├── search/            # 搜索引擎实现
│   └── elasticsearch/ # Elasticsearch 实现
└── other/             # 其他中间件实现
```

## 📨 消息队列 MQ

- [RabbitMQ 基础与应用](./mq-rabbitmq.md)
  - 消息模型与交换机类型
  - 消息确认机制
  - 死信队列与延迟队列
- [Kafka 原理与分区机制](./mq-kafka.md)
  - 分区与副本机制
  - 消息存储与清理
  - 消费者组与重平衡
- [MQ 消息可靠性保障方案](./mq-reliability.md)
  - 消息幂等性处理
  - 消息重试机制
  - 消息补偿方案

## 💾 缓存中间件

- [Redis 单机与集群部署](../database/redis-cluster.md)
  - 主从复制配置
  - 哨兵模式部署
  - 集群模式搭建
- [缓存一致性与双写问题](./redis-consistency.md)
  - 缓存更新策略
  - 双写一致性方案
  - 缓存穿透与雪崩防护

## 🔍 搜索引擎

- [Elasticsearch 基础概念](./elasticsearch-intro.md)
  - 索引与文档
  - 分词器与映射
  - 查询语法
- [ES 查询优化实践](./elasticsearch-optimization.md)
  - 索引优化
  - 查询性能调优
  - 集群监控

## 🔧 其他中间件

- [Nginx 高并发配置指南](./nginx.md)
  - 负载均衡配置
  - 反向代理设置
  - 性能优化参数
- [Zookeeper 注册中心实战](./zookeeper.md)
  - 集群部署
  - 数据同步机制
  - 选举算法

## 📚 最佳实践

### 消息队列选型建议

- 高吞吐量场景：Kafka
- 复杂路由场景：RabbitMQ
- 简单队列场景：Redis List

### 缓存使用建议

- 热点数据优先缓存
- 设置合理的过期时间
- 使用多级缓存架构

### 搜索引擎优化建议

- 合理设计索引结构
- 使用批量操作减少请求
- 定期优化索引性能

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进这个项目。在提交代码前，请确保：

1. 代码符合项目的编码规范
2. 添加必要的单元测试
3. 更新相关文档

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](../LICENSE) 文件
