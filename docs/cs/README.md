# 计算机科学基础指南

![Computer Science](https://img.shields.io/badge/Computer%20Science-Fundamentals-blueviolet)
![OS](https://img.shields.io/badge/OS-Linux-green)
![Network](https://img.shields.io/badge/Network-TCP/IP-blue)
![License](https://img.shields.io/badge/License-Apache%202.0-blue)

## 📋 目录

- [项目概述](#项目概述)
- [计算机组成原理](#计算机组成原理)
- [操作系统](#操作系统)
- [计算机网络](#计算机网络)
- [数据结构与算法](#数据结构与算法)
- [编译原理](#编译原理)
- [资源推荐](#资源推荐)
- [贡献指南](#贡献指南)

## 🎯 项目概述

本模块系统总结了计算机科学的核心基础知识，旨在帮助开发者：

- 深入理解计算机系统的工作原理
- 掌握操作系统和网络的核心概念
- 提升算法和数据结构能力
- 培养系统思维和问题解决能力
- 为高级技术学习打下坚实基础

## 🖥️ 计算机组成原理

### 数字逻辑

- [数字系统](./computer-architecture/number-systems.md)
  - 二进制与补码
  - 浮点数表示
  - 逻辑运算
- [组合逻辑电路](./computer-architecture/combinational-logic.md)
  - 门电路
  - 多路复用器
  - 加法器

### 处理器架构

- [CPU 工作原理](./computer-architecture/cpu-principle.md)
  - 指令集架构
  - 流水线技术
  - 超标量处理
- [存储体系](./computer-architecture/memory-hierarchy.md)
  - 寄存器
  - 高速缓存
  - 主存储器
  - 虚拟内存

### 输入输出系统

- [总线结构](./computer-architecture/bus-architecture.md)
  - 总线类型
  - 总线仲裁
  - 数据传输
- [I/O 系统](./computer-architecture/io-system.md)
  - 中断机制
  - DMA 传输
  - I/O 控制器

## 🧩 操作系统

### 进程管理

- [进程与线程](./os/process-thread.md)
  - 进程模型
  - 线程模型
  - 协程
- [进程调度](./os/process-scheduling.md)
  - 调度算法
  - 优先级调度
  - 实时调度

### 内存管理

- [内存分配](./os/memory-allocation.md)
  - 分页机制
  - 分段机制
  - 虚拟内存
- [页面置换](./os/page-replacement.md)
  - 置换算法
  - 工作集模型
  - 抖动处理

### 文件系统

- [文件管理](./os/file-system.md)
  - 文件组织
  - 目录结构
  - 文件操作
- [磁盘管理](./os/disk-management.md)
  - 磁盘调度
  - RAID 技术
  - 文件系统实现

## 🌐 计算机网络

### 网络基础

- [网络体系结构](./network/architecture.md)
  - OSI 模型
  - TCP/IP 模型
  - 协议栈
- [物理层](./network/physical-layer.md)
  - 传输介质
  - 编码技术
  - 复用技术

### 传输层

- [TCP 协议](./network/tcp.md)
  - 连接管理
  - 可靠传输
  - 流量控制
  - 拥塞控制
- [UDP 协议](./network/udp.md)
  - 特点与应用
  - 可靠性实现

### 应用层

- [HTTP 协议](./network/http.md)
  - 请求响应
  - 状态管理
  - 缓存机制
- [HTTPS 安全](./network/https.md)
  - TLS 协议
  - 证书机制
  - 加密算法

## 🧮 数据结构与算法

### 基础数据结构

- [线性结构](./algorithm/linear-structures.md)
  - 数组
  - 链表
  - 栈与队列
- [树形结构](./algorithm/tree-structures.md)
  - 二叉树
  - 平衡树
  - 堆

### 高级数据结构

- [图论基础](./algorithm/graph-theory.md)
  - 图的表示
  - 遍历算法
  - 最短路径
- [哈希表](./algorithm/hash-table.md)
  - 哈希函数
  - 冲突处理
  - 应用场景

### 算法设计

- [排序算法](./algorithm/sorting.md)
  - 比较排序
  - 非比较排序
  - 外部排序
- [动态规划](./algorithm/dynamic-programming.md)
  - 状态定义
  - 转移方程
  - 优化技巧

## 🔧 编译原理

### 编译器基础

- [词法分析](./compiler/lexical-analysis.md)
  - 正则表达式
  - 有限自动机
  - 词法分析器
- [语法分析](./compiler/syntax-analysis.md)
  - 上下文无关文法
  - 语法树
  - 语法分析器

### 代码生成

- [中间代码](./compiler/intermediate-code.md)
  - 三地址码
  - 静态单赋值
  - 优化技术
- [目标代码](./compiler/target-code.md)
  - 指令选择
  - 寄存器分配
  - 代码优化

## 📚 资源推荐

### 官方文档

- [Linux 内核文档](https://www.kernel.org/doc/)
- [TCP/IP 协议规范](https://tools.ietf.org/html/rfc793)
- [HTTP 协议规范](https://tools.ietf.org/html/rfc2616)

### 学习资源

- 《计算机组成与设计：硬件/软件接口》
- 《现代操作系统》
- 《计算机网络：自顶向下方法》
- 《算法导论》

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进这个项目。在提交内容前，请确保：

1. 内容准确且经过验证
2. 提供完整的示例代码
3. 保持文档格式统一
4. 添加必要的参考资料

## 📄 许可证

本项目采用 Apache 2.0 许可证 - 详见 [LICENSE](../LICENSE) 文件
