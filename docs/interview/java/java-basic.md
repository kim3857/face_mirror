# Java核心知识面试指南

## 目录
- [集合框架](#集合框架)
- [并发编程](#并发编程)
- [JVM原理与调优](#jvm原理与调优)

## 集合框架

### 1. 基础集合类
- ArrayList
  - 扩容机制
  - 线程安全性
  - 性能分析
- LinkedList
  - 实现原理
  - 适用场景
  - 性能分析
- HashMap
  - 数据结构
  - 哈希冲突处理
  - 扩容机制
  - 线程安全性

### 2. 并发集合
- ConcurrentHashMap
  - 实现原理
  - 分段锁机制
  - 性能优化
- CopyOnWriteArrayList
  - 实现原理
  - 适用场景
  - 性能分析

### 3. 集合工具类
- Collections
- Arrays
- Stream API

## 并发编程

### 1. 线程基础
- 线程生命周期
- 线程状态转换
- 线程创建方式
- 线程优先级

### 2. 线程同步
- synchronized
  - 实现原理
  - 锁升级过程
  - 使用场景
- volatile
  - 内存可见性
  - 禁止指令重排序
- Lock
  - ReentrantLock
  - ReadWriteLock
  - Condition

### 3. 线程池
- ThreadPoolExecutor
  - 核心参数
  - 工作流程
  - 拒绝策略
- Executors
  - 常用工厂方法
  - 使用建议

### 4. 并发工具类
- CountDownLatch
- CyclicBarrier
- Semaphore
- Exchanger

## JVM原理与调优

### 1. 内存模型
- 程序计数器
- 虚拟机栈
- 本地方法栈
- 堆
- 方法区

### 2. 垃圾回收
- 垃圾回收算法
  - 标记-清除
  - 复制算法
  - 标记-整理
- 垃圾收集器
  - Serial
  - ParNew
  - CMS
  - G1
  - ZGC

### 3. 类加载机制
- 类加载过程
- 双亲委派模型
- 类加载器
- 自定义类加载器

### 4. JVM调优
- 内存调优
- GC调优
- 线程调优
- 性能监控

## 面试常见问题

### 1. 集合相关
- HashMap和HashTable的区别
- ArrayList和LinkedList的区别
- ConcurrentHashMap的实现原理

### 2. 并发相关
- synchronized和Lock的区别
- volatile的作用和原理
- 线程池的工作原理

### 3. JVM相关
- JVM内存模型
- 垃圾回收算法
- 类加载机制

## 面试技巧

### 1. 答题策略
- 从源码角度分析
- 结合实际应用场景
- 展示性能优化思路

### 2. 注意事项
- 注意回答的深度
- 保持逻辑清晰
- 适当举例说明

## 参考资料
- 《Java并发编程实战》
- 《深入理解Java虚拟机》
- 《Java核心技术》 