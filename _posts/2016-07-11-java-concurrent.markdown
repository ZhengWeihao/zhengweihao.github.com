---
layout: index
title: Java并发编程笔记
date: 2016-07-11 17:42:18
categories: [java, concurrent]
---

Java并发编程笔记
---

* 基本概念：
   * 进程：程序
   * 线程：进程中负责执行程序的执行单元
   * 并发：以可独立执行的进程集合的方式编程
   * 并行：以可同时执行的计算指令方式编程
* 多线程实现原理：
   * 时间片调度


* 优点：
  * 提高资源利用率
  * 提高程序性能
* 风险：
  * 死锁
  * 上下文切换
  * 线程安全
* 实现方式：
  * 继承Thread类：单继承
  * 实现Runnable接口：多实现
  * ExecutorService
  * Callable接口、Future类实现带返回值的多线程操作
* 生命周期：
  * 创建：new Thread()
  * 就绪：start()
  * 运行：CPU调度，根据时间片轮询或yield方法切换就绪态
  * 阻塞：sleep()/join()
  * 终止：run结束或者Main结束
  * 锁定：synchronized
  * 等待：wait/notify
* 关键问题：
  * 线程之间通信（线程之间交换信息）
    * 共享内存
    * 消息传递：wait/notify/notifyAll
  * 线程同步（控制不同线程操作发生的相对顺序）
    * 共享内存的并发模型中，同步是显式进行，如：synchronized
    * 消息传递的并发模型中，消息发送必须在消息接收之前，所以同步是隐式的：
      * 可见性问题：
        * volatile、synchronized
  * 控制线程执行的顺序：
    * join：让主线程等待子线程结束后才能继续运行
    * ExecutorService executor = Excutors.newSingleThreadExecutor();// 只有一个线程的线程池，操作不限数量的线程队列（FIFO队列 -> 排队处理）


* volatile关键字
  * 支持：可见性
  * 不支持：复合操作的原子性
  * 实现原理：
    * 对volatile变量进行写操作时，JVM向处理器发送一条Lock前缀指令。会把缓存行的数据写会系统内存
    * 在多处理器的情况下，保证各处理器缓存一致性，实现缓存一致性协议（设置失效与更新）
  * 优化：拒绝指令重排
* synchronized关键字：锁机制，隐式释放
  * 支持：重入性、互斥性、可见性
  * 不支持：超时等待、可中断性、公平性
  * 实现原理：
    * monitorenter、monitorexit字节码指令
    * SynchronizedQueue（存储无法获取锁的线程）
  * 释放时机：
    * 代码块执行完成
    * 线程执行出现异常
* Lock接口：显式获取、释放锁，更适合竞争激烈场景（Java5推出的juc包中）
  * 重入锁
  * 读写锁
  * 可中断锁
  * 公平锁
  * 可重入读写锁
* 分布式锁：
  * 场景：
    * 多项服务访问同个资源
  * 实现方式：
    * 数据库：
      * 原理：
        * lock表.method_name字段，增加唯一性约束，实现获取锁功能
      * 缺点：
        * 记录删除失败
        * 不支持重入性
    * ZooKeeper：
      * 原理：
        * /Locks目录中写入methodName临时有序节点，当前最小节点获得锁
      * 优点：
        * Watch机制自动删除失效节点
    * Redis：
      * 原理：
        * setnx命令：在key不存在时设置值，并返回0或1
* 死锁：
  * 原因：
    * 多把锁（哲学家问题）
  * 解决方式：
    * 全局顺序
    * 超时时间（不能避免死锁）
    * 可中断锁（不能避免死锁）
* 无锁化：
  * 优点：
    * 减少上下文切换
    * 无须手动获取、释放锁
    * 通用、高效、成熟
  * 方法：
    * Hash取模分段处理
    * Atomic类
    * ReidsAtomic类（适用分布式环境）
* 代码实践：
  * [几种锁的使用及哲学家问题演示代码](https://github.com/ZhengWeihao/JavaDemo/tree/master/src/main/java/com/zhengweihao/lock)
* 参考资料
  * [并发不是并行，它更好！](http://www.iteye.com/news/28915)