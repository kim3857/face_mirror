## ☕ Java 基础模块说明

> 本模块系统整理了 Java 开发的核心基础，包括语言特性、集合框架、并发编程、JVM 原理、异常体系等。适用于 Java 初中高级工程师自查补漏、深入学习。

------

### 📚 模块结构导航

| 分类                  | 内容简介                                               |
| --------------------- | ------------------------------------------------------ |
| `01-java-basic.md`    | Java 基本语法、关键字、数据类型、运算符、流程控制      |
| `02-oop.md`           | 面向对象三大特性（封装、继承、多态）、抽象类与接口     |
| `03-collection.md`    | Java 容器体系（List、Set、Map）、底层原理与比较        |
| `04-generic.md`       | 泛型语法、擦除机制、类型通配符使用实践                 |
| `05-exception.md`     | Java 异常体系（Checked/Unchecked）、异常链             |
| `06-io.md`            | Java IO/NIO 基础、字节流/字符流、Buffer/Channel        |
| `07-thread.md`        | 线程生命周期、Runnable vs Callable、线程池详解         |
| `08-concurrent.md`    | 并发原理、synchronized、Lock、CAS、并发容器            |
| `09-jvm.md`           | JVM 内存结构、垃圾回收、类加载机制、调优实战           |
| `10-java8-feature.md` | Lambda 表达式、Stream、Optional、时间 API              |
| `11-java17-new.md`    | Java 11~17 新特性：Record、sealed、pattern matching 等 |



------

### ✅ 学习建议

- **建议顺序阅读**：由基础语法逐步过渡到并发与 JVM，形成知识闭环。
- **结合实践**：推荐结合简单项目或力扣算法题加深基础掌握。
- **关注源码实现**：如 ArrayList、HashMap、ReentrantLock 建议源码级别理解。

------

### 📌 补充资料推荐

- 《Java 编程思想》
- 《Effective Java》
- 《深入理解 Java 虚拟机》
- 官方文档：https://docs.oracle.com/javase/8/docs/

------

### 🧠 知识自检

✅ 你是否能清楚回答以下问题？

- Java 中的值传递与引用传递？
- HashMap 如何解决 hash 冲突？
- ThreadLocal 的内存泄露风险来源？
- JVM GC 日志怎么看？如何调优？
- Java 中 volatile 和 synchronized 有何异同？