# DevOps 实践指南

![CI/CD](https://img.shields.io/badge/CI/CD-GitHub%20Actions-blue)
![Docker](https://img.shields.io/badge/Docker-24.0-green)
![Kubernetes](https://img.shields.io/badge/K8s-1.28-blue)
![License](https://img.shields.io/badge/License-Apache%202.0-blue)

## 📋 目录
- [项目概述](#项目概述)
- [持续集成与部署](#持续集成与部署)
- [容器化与编排](#容器化与编排)
- [监控与告警](#监控与告警)
- [日志管理](#日志管理)
- [基础设施即代码](#基础设施即代码)
- [安全与合规](#安全与合规)
- [资源推荐](#资源推荐)
- [贡献指南](#贡献指南)

## 🎯 项目概述

本模块系统总结了 DevOps 实践的核心知识与最佳实践，旨在帮助团队：

- 实现高效的持续集成与部署
- 构建可靠的容器化基础设施
- 建立完善的监控与告警体系
- 确保系统安全与合规性
- 提升团队协作效率

## 🔄 持续集成与部署

### CI/CD 工具链
- [Jenkins 实践指南](./ci-cd/jenkins.md)
  - 流水线设计
  - 多环境部署
  - 自动化测试集成
- [GitHub Actions 实战](./ci-cd/github-actions.md)
  - 工作流配置
  - 自定义 Action
  - 密钥管理
- [GitLab CI/CD 实践](./ci-cd/gitlab-ci.md)
  - 流水线优化
  - 制品管理
  - 环境管理

### 构建与部署
- [Maven/Gradle 构建优化](./ci-cd/build-optimization.md)
- [Docker 镜像构建最佳实践](./ci-cd/docker-build.md)
- [蓝绿部署与金丝雀发布](./ci-cd/deployment-strategies.md)

## 🐳 容器化与编排

### Docker
- [容器化最佳实践](./container/docker-best-practices.md)
  - 镜像优化
  - 网络配置
  - 存储管理
- [Docker Compose 实战](./container/docker-compose.md)
  - 多容器编排
  - 环境变量管理
  - 服务依赖

### Kubernetes
- [集群部署与配置](./k8s/cluster-deployment.md)
  - 高可用部署
  - 网络配置
  - 存储方案
- [工作负载管理](./k8s/workload-management.md)
  - Deployment 配置
  - StatefulSet 实践
  - DaemonSet 应用
- [服务发现与负载均衡](./k8s/service-discovery.md)
  - Ingress 配置
  - Service 类型
  - 负载均衡策略

## 📊 监控与告警

### 监控系统
- [Prometheus 实战](./monitoring/prometheus.md)
  - 指标收集
  - 告警规则
  - 数据可视化
- [Grafana 仪表板设计](./monitoring/grafana.md)
  - 面板配置
  - 告警通知
  - 数据源管理

### 应用性能监控
- [APM 工具使用](./monitoring/apm.md)
  - 链路追踪
  - 性能分析
  - 资源监控

## 📝 日志管理

### 日志收集与分析
- [ELK Stack 实践](./logging/elk-stack.md)
  - Logstash 配置
  - Elasticsearch 优化
  - Kibana 可视化
- [Fluentd 日志收集](./logging/fluentd.md)
  - 日志路由
  - 数据转换
  - 输出配置

## 🏗 基础设施即代码

### 配置管理
- [Ansible 自动化运维](./iac/ansible.md)
  - Playbook 编写
  - 角色管理
  - 变量控制
- [Terraform 云资源管理](./iac/terraform.md)
  - 资源编排
  - 状态管理
  - 模块化设计

## 🔒 安全与合规

### 安全实践
- [容器安全](./security/container-security.md)
  - 镜像扫描
  - 运行时保护
  - 权限控制
- [CI/CD 安全](./security/ci-cd-security.md)
  - 密钥管理
  - 访问控制
  - 审计日志

### 合规管理
- [安全策略配置](./security/security-policies.md)
- [合规检查工具](./security/compliance-tools.md)

## 📚 资源推荐

### 官方文档
- [Docker 官方文档](https://docs.docker.com/)
- [Kubernetes 官方文档](https://kubernetes.io/docs/)
- [Jenkins 官方文档](https://www.jenkins.io/doc/)
- [Prometheus 官方文档](https://prometheus.io/docs/)

### 学习资源
- 经典书籍推荐
- 技术博客推荐
- 视频课程推荐

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进这个项目。在提交内容前，请确保：

1. 内容准确且经过验证
2. 提供完整的示例代码
3. 保持文档格式统一
4. 添加必要的参考资料

## 📄 许可证

本项目采用 Apache 2.0 许可证 - 详见 [LICENSE](../LICENSE) 文件

# 中间件模块导航

本模块聚焦常用中间件的原理、使用与优化，包括消息队列、缓存、搜索引擎等。

## 📨 消息队列 MQ
- [RabbitMQ 基础与应用](./mq-rabbitmq.md)
- [Kafka 原理与分区机制](./mq-kafka.md)
- [MQ 消息可靠性保障方案](./mq-reliability.md)

## 💾 缓存中间件
- [Redis 单机与集群部署](../database/redis-cluster.md)
- [缓存一致性与双写问题](./redis-consistency.md)

## 🔍 搜索引擎
- [Elasticsearch 基础概念](./elasticsearch-intro.md)
- [ES 查询优化实践](./elasticsearch-optimization.md)

## 🔧 其他中间件
- [Nginx 高并发配置指南](./nginx.md)
- [Zookeeper 注册中心实战](./zookeeper.md)
