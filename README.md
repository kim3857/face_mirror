# face_mirror

本仓库汇总了 Java 后端工程师核心知识体系，覆盖 Java 基础、并发编程、JVM、常用框架、数据库、微服务、分布式系统、消息中间件、DevOps 等方向，适合日常复习与系统性学习。

---

## 📚 目录

1. [Java 基础](#java-基础)
2. [集合与数据结构](#集合与数据结构)
3. [并发编程](#并发编程)
4. [JVM 虚拟机](#jvm-虚拟机)
5. [常用框架](#常用框架)
6. [数据库](#数据库)
7. [Spring 全家桶](#spring-全家桶)
8. [微服务架构](#微服务架构)
9. [中间件](#中间件)
10. [系统设计与分布式](#系统设计与分布式)
11. [DevOps 与运维](#devops-与运维)
12. [面试题精选](#面试题精选)

---

## Java 基础

- 面向对象：继承、封装、多态、接口、抽象类
- 异常机制：Checked 与 Unchecked 异常，异常链
- 关键字：final、static、this、super、transient、volatile、synchronized
- 包与访问控制：private / protected / public / default
- 泛型、反射、注解
- Lambda 表达式、Stream API（Java 8+）

---

## 集合与数据结构

- List、Set、Map、Queue 的常见实现类及底层原理
- HashMap、ConcurrentHashMap、TreeMap、LinkedHashMap
- ArrayList vs LinkedList 区别
- fail-fast 与 fail-safe 机制
- Collections 工具类和 Arrays 类常用方法

---

## 并发编程

- 线程的创建与线程池（ThreadPoolExecutor）
- synchronized、Lock、ReentrantLock、ReadWriteLock
- volatile、happens-before、JMM 内存模型
- 原子类（AtomicInteger 等）
- 并发工具类：CountDownLatch、CyclicBarrier、Semaphore、Future、CompletableFuture
- 死锁、活锁、饥饿

---

## JVM 虚拟机

- JVM 内存结构（堆、栈、方法区、元空间）
- 类加载过程：加载、验证、准备、解析、初始化
- 垃圾回收器：Serial、Parallel、CMS、G1、ZGC 简介
- GC Roots、可达性分析
- JVM 参数调优、内存泄漏排查

---

## 常用框架

- Spring Framework：IOC、AOP、事务、事件
- Spring Boot：自动装配原理，starter，配置管理
- Spring MVC：拦截器、参数解析器、返回值处理器
- MyBatis：SQL 映射文件、缓存、插件机制
- Hibernate（如有）

---

## 数据库

- SQL 基础、事务四大特性（ACID）
- 索引原理（B+ 树）、覆盖索引、联合索引、最左匹配
- 数据库优化：慢查询分析、执行计划（EXPLAIN）
- 分库分表策略、读写分离
- Redis 基础数据结构、持久化、事务、过期策略

---

## Spring 全家桶

- Spring Security 权限认证流程
- Spring Cloud：Eureka、Nacos、Gateway、Feign、Ribbon、Hystrix、Sleuth、Config
- Spring Boot Actuator、监控指标采集
- 分布式配置中心、熔断、限流、链路追踪

---

## 微服务架构

- 单体 vs 微服务优缺点
- 服务注册与发现
- 接口调用：RESTful、gRPC、Dubbo
- API 网关：统一认证鉴权、流控、路由
- 服务容错：重试、超时、限流、熔断
- 配置中心、分布式事务（Seata/Saga）

---

## 中间件

- RabbitMQ / Kafka：消息模型、消费模式、消息确认与重试
- Elasticsearch：倒排索引、搜索原理
- Redis：缓存穿透、雪崩、击穿
- Nginx：负载均衡、反向代理

---

## 系统设计与分布式

- CAP 原理、BASE 理论
- 分布式锁实现方案（Redis、ZK）
- 一致性哈希、雪花算法（ID 生成）
- 限流算法：令牌桶、漏桶
- 幂等性处理方案

---

## DevOps 与运维

- 日志采集：Logback + ELK / EFK
- 链路追踪：Zipkin、Jaeger
- CI/CD：Jenkins、GitLab CI、Sonar
- 容器化与部署：Docker、Kubernetes、Helm

---

## 面试题精选

- Java 基础 100 问
- JVM 原理与调优面试题
- 并发场景模拟题与代码示例
- 分布式系统常见问题：CAP/事务/一致性/锁
- 框架底层原理手写题

---

## 📌 说明

- 本文档适合用于日常知识巩固、面试冲刺复习。
- 欢迎 PR 或提交 issue 进行补充。

---

## 📮 联系我

如需获取详细笔记或面试题答案，可联系：
- 📧 邮箱：tonyliu1314521@gmail.com
- 📦 GitHub: https://github.com/kim3857/face_mirror.git
- 📦 Gitee: https://gitee.com/mr_tony/face_mirror.git
- 📦 Gitcode: https://gitcode.com/gcw_fDRVFVXe/face_mirror.git