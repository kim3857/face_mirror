# Spring生态面试指南

## 目录
- [Spring核心原理](#spring核心原理)
- [Spring Boot自动配置](#spring-boot自动配置)
- [Spring Cloud微服务](#spring-cloud微服务)

## Spring核心原理

### 1. IOC容器
- Bean生命周期
  - 实例化
  - 属性注入
  - 初始化
  - 销毁
- 依赖注入
  - 构造器注入
  - Setter注入
  - 字段注入
- Bean作用域
  - Singleton
  - Prototype
  - Request
  - Session

### 2. AOP原理
- 代理模式
  - JDK动态代理
  - CGLIB代理
- 切面编程
  - 切点表达式
  - 通知类型
  - 织入时机
- 事务管理
  - 事务传播行为
  - 事务隔离级别
  - 事务回滚规则

### 3. Spring MVC
- 请求处理流程
- 参数绑定机制
- 视图解析器
- 异常处理

## Spring Boot自动配置

### 1. 自动配置原理
- @SpringBootApplication
- @EnableAutoConfiguration
- 条件注解
- 配置属性

### 2. 常用功能
- 数据访问
  - JPA
  - MyBatis
  - Redis
- 消息队列
  - RabbitMQ
  - Kafka
- 安全框架
  - Spring Security
  - OAuth2

### 3. 性能优化
- 启动优化
- 内存优化
- 数据库优化
- 缓存优化

## Spring Cloud微服务

### 1. 服务注册与发现
- Eureka
  - 服务注册
  - 服务发现
  - 心跳机制
- Nacos
  - 配置管理
  - 服务管理
  - 动态配置

### 2. 服务调用
- OpenFeign
  - 接口定义
  - 负载均衡
  - 熔断降级
- Ribbon
  - 负载均衡策略
  - 重试机制
  - 超时控制

### 3. 服务网关
- Gateway
  - 路由配置
  - 过滤器
  - 限流熔断
- Zuul
  - 路由规则
  - 过滤器链
  - 性能优化

### 4. 分布式配置
- Config
  - 配置中心
  - 配置刷新
  - 版本管理
- Apollo
  - 配置管理
  - 灰度发布
  - 权限控制

## 面试常见问题

### 1. Spring相关
- IOC和AOP的原理
- Bean的生命周期
- Spring事务管理

### 2. Spring Boot相关
- 自动配置原理
- 启动流程
- 常用注解

### 3. Spring Cloud相关
- 服务注册发现原理
- 服务调用方式
- 分布式事务处理

## 面试技巧

### 1. 答题策略
- 从源码角度分析
- 结合实际项目经验
- 展示架构设计能力

### 2. 注意事项
- 注意回答的深度
- 保持逻辑清晰
- 适当举例说明

## 参考资料
- 《Spring实战》
- 《Spring Boot实战》
- 《Spring Cloud微服务实战》 