<h1 align="center">🪞 Face-Mirror · 面镜</h1>
<p align="center">一面照出知识短板与能力盲区的“照妖镜”，系统整理后端核心技术，助你升职加薪不再迷茫。</p>

---

## 📚 项目介绍

> **“不是面经，而是一面镜子。”**

在浩瀚如海的技术世界中，作为一名后端开发者，我们每天都在面对一个终极难题——如何系统地学习，如何高效地进阶。而“面镜”，正是为了解决这个痛点而诞生的。

这是我自己的“照妖镜”，一面能照出自身短板与知识死角的镜子，也是一个为所有程序员朋友准备的免费、开源的知识体系整理项目。在这里，没有东拼西凑的碎片化内容，也不需要付费订阅，所有内容全都开放，持续更新。

**面镜** 涵盖后端开发核心技能，包括但不限于 Java、Spring、MySQL、Redis、消息队列、分布式、微服务、系统设计、安全与性能优化等内容，力求打造一站式技术导航图谱，帮助你从容应对工作挑战、技术面试，乃至职场晋升。

希望这面镜子不仅能帮助我自身精进，也能为每一位在技术之路上跋涉的你，点亮前行的方向。

内容如有疏漏或错误之处，欢迎大家批正指教。

---

## 🧭 目录导航

| 分类                                         | 描述                                 |
| -------------------------------------------- | ------------------------------------ |
| 🧠 [Java 基础](docs/java/README.md)           | 语法、集合、JVM、并发                |
| 🧱 [计算机基础](docs/cs/README.md)            | 操作系统、网络、数据结构             |
| 🌐 [Web 后端框架](docs/frameworks/README.md)  | Spring 全家桶、MyBatis、Servlet      |
| ⚙️ [微服务架构](docs/microservices/README.md) | Spring Cloud、Dubbo、Nacos、Sentinel |
| 🚀 [高性能中间件](docs/middleware/README.md)  | Redis、Kafka、RabbitMQ、ES           |
| 📦 [数据库](docs/db/README.md)                | MySQL、SQL优化、事务、索引           |
| 🔧 [DevOps 与运维](docs/devops/README.md)     | Docker、K8s、CI/CD、监控告警         |
| 🧪 [系统设计与架构](docs/design/README.md)    | 常用架构图、系统拆分、高可用         |
| 🛡️ [安全与认证](docs/security/README.md)      | 登录鉴权、XSS、JWT、HTTPS            |
| 📈 [面试专题](docs/interview/README.md)       | 常见八股文、系统设计题、行为面       |

---

## 🧩 项目结构梳理

```bash
face-mirror/
├── docs/                      # 所有文档资料
│   ├── java/                  # Java基础、JVM、并发
│   ├── cs/                    # 操作系统、网络等计算机基础
│   ├── frameworks/            # Spring Boot, MyBatis 等
│   ├── microservices/         # 微服务相关
│   ├── middleware/            # Redis, MQ, ES 等
│   ├── db/                    # MySQL 等数据库相关
│   ├── devops/                # Docker, K8s 等部署运维
│   ├── design/                # 系统设计
│   ├── security/              # 安全相关
│   └── interview/             # 面试相关内容
├── assets/                    # 图片资源、架构图
└── README.md                  # 项目介绍
```

## 🔖 特色亮点

- ✅ 全部免费开源，拒绝付费割韭菜
- 🧩 模块清晰分类，查漏补缺效率高
- 🧠 包含原理讲解 + 实战案例 + 面试准备
- 🛠️ 持续更新维护中，欢迎提 Issue/PR

1. ](#面试题精选)

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

## 🫶 支持与反馈

- 📌 欢迎 Star 🌟 收藏支持
- 🧵 有问题欢迎提 [Issues](https://github.com/你的项目/issues)
- 💬 加入我们，一起完善这面“面镜”

------

## ⭐ 致谢

感谢所有为这个项目提出建议或贡献内容的朋友。
 希望这面镜子，不仅能照亮我自己，也能照亮你前行的方向。



