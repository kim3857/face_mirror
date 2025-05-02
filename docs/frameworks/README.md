# 主流框架技术栈指南

![Spring](https://img.shields.io/badge/Spring-6.0-green)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2-brightgreen)
![MyBatis](https://img.shields.io/badge/MyBatis-3.5-blue)
![Dubbo](https://img.shields.io/badge/Dubbo-3.2-orange)
![License](https://img.shields.io/badge/License-Apache%202.0-blue)

## 📋 目录
- [项目概述](#项目概述)
- [Spring 生态](#spring-生态)
- [ORM 框架](#orm-框架)
- [RPC 框架](#rpc-框架)
- [Web 框架](#web-框架)
- [微服务框架](#微服务框架)
- [资源推荐](#资源推荐)
- [贡献指南](#贡献指南)

## 🎯 项目概述

本模块系统总结了主流框架的核心知识与应用实践，旨在帮助开发者：

- 深入理解各框架的设计原理
- 掌握框架的最佳实践
- 解决实际开发中的常见问题
- 提升框架选型与架构设计能力

## 🌱 Spring 生态

### Spring Framework
- [IOC 与 DI 原理](./spring/ioc-di.md)
- [Bean 生命周期详解](./spring/bean-lifecycle.md)
- [AOP 原理及应用](./spring/aop.md)
- [事务管理机制](./spring/transaction.md)

### Spring MVC
- [请求流程](./spring/mvc-request-flow.md)
- [参数绑定与校验](./spring/mvc-binding-validation.md)
- [异常处理机制](./spring/mvc-exception.md)

### Spring Boot
- [自动配置机制原理](./spring/spring-boot-autoconfig.md)
- [Starter 模式解析](./spring/spring-boot-starter.md)
- [配置文件详解](./spring/spring-boot-config.md)
- [监控与健康检查](./spring/spring-boot-actuator.md)

### Spring Security
- [认证与授权](./spring/spring-security-auth.md)
- [JWT 认证流程](./spring/spring-security-jwt.md)
- [OAuth2 集成](./spring/spring-security-oauth2.md)

## 💾 ORM 框架

### MyBatis
- [核心原理](./mybatis/core-principle.md)
- [动态 SQL](./mybatis/dynamic-sql.md)
- [缓存机制](./mybatis/cache.md)
- [插件开发](./mybatis/plugin.md)

### JPA/Hibernate
- [实体映射](./jpa/entity-mapping.md)
- [查询优化](./jpa/query-optimization.md)
- [缓存策略](./jpa/cache-strategy.md)

## 🔄 RPC 框架

### Dubbo
- [服务注册与发现](./dubbo/service-discovery.md)
- [负载均衡策略](./dubbo/load-balance.md)
- [集群容错](./dubbo/cluster-fault-tolerance.md)
- [服务治理](./dubbo/service-governance.md)

### gRPC
- [Protocol Buffers](./grpc/protocol-buffers.md)
- [服务定义与实现](./grpc/service-definition.md)
- [流式处理](./grpc/streaming.md)

## 🌐 Web 框架

### Spring WebFlux
- [响应式编程](./webflux/reactive-programming.md)
- [WebClient 使用](./webflux/webclient.md)
- [性能优化](./webflux/performance.md)

### Vert.x
- [事件驱动模型](./vertx/event-driven.md)
- [异步编程](./vertx/async-programming.md)
- [集群部署](./vertx/cluster.md)

## ☁️ 微服务框架

### Spring Cloud
- [服务注册与发现](./spring-cloud/service-discovery.md)
- [配置中心](./spring-cloud/config-center.md)
- [网关服务](./spring-cloud/gateway.md)
- [熔断降级](./spring-cloud/circuit-breaker.md)

### Spring Cloud Alibaba
- [Nacos 使用](./spring-cloud-alibaba/nacos.md)
- [Sentinel 限流](./spring-cloud-alibaba/sentinel.md)
- [Seata 分布式事务](./spring-cloud-alibaba/seata.md)

## 📚 资源推荐

### 官方文档
- [Spring 官方文档](https://docs.spring.io/)
- [MyBatis 官方文档](https://mybatis.org/mybatis-3/)
- [Dubbo 官方文档](https://dubbo.apache.org/)
- [gRPC 官方文档](https://grpc.io/)

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