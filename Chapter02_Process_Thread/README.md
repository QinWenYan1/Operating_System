# 第2章 进程与线程 (Process & Thread)

> **章节定位**：从 CPU 管理的直观想法出发，逐步构建多进程图像、线程机制、CPU 调度、进程同步与死锁处理的完整知识链，是操作系统最核心的基础章节。

---

## 📋 章节导航与重难点

### 2.1 [CPU管理的直观想法 (Intuitive Ideas of CPU Management)](./2.1_Intuitive_Ideas_of_CPU_Management.md)
- **核心**：
  - 冯·诺依曼架构(Von Neumann Architecture)的取指-执行循环
  - CPU 管理的核心问题：如何让 CPU 高效运转
  - 从单道程序到多道程序的直观演进
- **难点**：
  - 程序计数器(PC)与指令寄存器(IR)的协作关系容易混淆
  - 为什么需要管理 CPU 而不是让程序自己跑

### 2.2 [多进程图像 (Multiple Processes)](./2.2_Multiple_Processes.md)
- **核心**：
  - 进程(Process)的定义与组成：代码段、数据段、栈、PCB
  - 进程三态模型：运行态(Running)、就绪态(Ready)、阻塞态(Blocked)
  - 多进程并发执行的直观图像与状态转换触发条件
- **难点**：
  - 区分并发(Concurrency)与并行(Parallelism)的概念差异
  - 理解 PCB(Process Control Block)为什么是进程存在的唯一标志

### 2.3 [用户级线程 (User Level Threads)](./2.3_User_Level_Threads.md)
- **核心**：
  - 线程(Thread)引入动机：轻量级并发单元
  - 用户级线程(ULT, User Level Thread)的实现机制：用户态库管理
  - 线程切换只需改栈指针和 PC，无需进入内核
- **难点**：
  - 用户级线程遇到阻塞系统调用时整个进程被挂起的致命缺陷
  - 为什么用户级线程对操作系统内核不可见

### 2.4 [内核级线程 (Kernel Threads)](./2.4_Kernel_Threads.md)
- **核心**：
  - 内核级线程(KLT, Kernel Level Thread)定义：OS 内核直接感知和管理
  - KLT 与 ULT 的核心区别：调度者不同、阻塞影响范围不同
  - KLT 的调度由操作系统统一完成，可并行运行于多核
- **难点**：
  - 区分用户级线程与内核级线程的调度主体差异
  - 理解内核级线程切换需要进入内核态的代价

### 2.5 [核心级线程实现实例 (Kernel Thread Implementation)](./2.5_Kernel_Thread_Implementation.md)
- **核心**：
  - Linux 0.11 线程实现机制：`task_struct` 与 TSS(Task State Segment)
  - 线程切换的硬件支持：TR 寄存器与 TSS 段
  - 内核栈(Kernel Stack)切换的具体过程
- **难点**：
  - 理解 `task_struct` 中内核栈指针与 TSS 的关联
  - 线程切换时现场保存与恢复的具体汇编级操作

### 2.6 [操作系统的那棵树 (The Tree of OS)](./2.6_The_Tree_Of_Os.md)
- **核心**：
  - 操作系统核心组件的层级结构：进程管理、内存管理、文件系统
  - 操作系统设计的树形视角：根为硬件抽象，叶为系统调用接口
  - 进程管理在 OS 树中的核心位置
- **难点**：
  - 抽象理解操作系统各模块的层次依赖关系
  - 区分系统调用层与内核服务层的边界

### 2.7 [CPU调度策略 (CPU Scheduling)](./2.7_CPU_Scheduling.md)
- **核心**：
  - 调度三目标：周转时间(Turnaround Time)、吞吐量(Throughput)、响应时间(Response Time)
  - 四大经典算法：FCFS、SJF、RR(Round Robin)、优先级调度(Priority Scheduling)
  - I/O 约束型(I/O-bound)与 CPU 约束型(CPU-bound)任务的特征差异
- **难点**：
  - 理解三个调度目标之间的内在矛盾：响应时间 vs 吞吐量
  - 多队列调度中前台任务饿死后台任务的优先级反转问题

### 2.8 [一个实际的schedule函数 (A Practical Schedule Function)](./2.8_A_Practical_Schedule_Function.md)
- **核心**：
  - Linux 0.11 `schedule()` 源码解析：遍历 `task[]` 数组找最大 `counter`
  - `counter` 的双重作用：时间片(Time Slice) + 动态优先级(Dynamic Priority)
  - `counter` 重新计算公式：`counter = counter/2 + priority`
- **难点**：
  - 理解 `counter` 如何同时实现 RR 轮转和 I/O 进程优先两个目标
  - 为什么阻塞态进程就绪后 `counter` 会大于非阻塞进程

### 2.9 [进程同步与信号量 (Processes Synchronization and Semaphore)](./2.9_Processes_Synchronization_And_Semaphore.md)
- **核心**：
  - 进程合作(Process Cooperation)与同步需求：司机-售票员问题
  - 生产者-消费者(Producer-Consumer)问题的信号量解法
  - 信号量(Semaphore)定义：P/V 操作、`value` 与 `PCB queue`
- **难点**：
  - 区分同步信号量(`full`/`empty`)与互斥信号量(`mutex`)的作用
  - 理解 P 操作必须先申请同步信号量再申请互斥信号量的顺序约束

### 2.10 [信号量临界区保护 (Critical Section)](./2.10_Critical_Section.md)
- **核心**：
  - 竞争条件(Race Condition)：多进程并发访问共享数据的时序错误
  - 临界区(Critical Section)定义与三条保护原则：互斥、有空让进、有限等待
  - 软件解法(Peterson、面包店算法)与硬件解法(TestAndSet 原子指令)
- **难点**：
  - Peterson 算法中 `flag` 与 `turn` 的协同逻辑：先举手后谦让
  - 面包店算法(Bakery Algorithm)中 `choosing` 数组的必要性
  - 为什么 `TestAndSet` 的 `while` 循环是忙等待(Busy Waiting)

### 2.11 [信号量的代码实现 (Coding Semaphore)](./2.11_coding_semaphore.md)
- **核心**：
  - `semtable` 数据结构：内核中的全局信号量表
  - `sys_sem_wait` / `sys_sem_post` 系统调用的 `cli/sti` 原子保护
  - Linux 0.11 `sleep_on` 隐式队列机制：栈上 `tmp` 变量形成链表
- **难点**：
  - `sleep_on` 的隐式队列设计：`tmp` 存在当前进程内核栈上，通过 TSS 恢复保持链
  - 级联唤醒(Cascade Wake-up)的 LIFO 顺序与 `wake_up` 只唤醒队首的协作
  - 为什么 `lock_buffer` 中用 `while(lock)` 而不是 `if(lock)`

### 2.12 [死锁处理 (Deadlock Handling)](./2.12_Deadlock_Handling.md)
- **核心**：
  - 死锁(Deadlock)四条件：互斥、不可抢占、请求和保持、循环等待
  - 死锁四策略对比：预防 / 避免 / 检测+恢复 / 忽略
  - 银行家算法(Banker's Algorithm)：安全序列(Safe Sequence)判定
- **难点**：
  - 银行家算法的安全性检查：模拟分配后寻找安全序列的推演过程
  - 理解安全状态(Safe State)与不死锁状态的包含关系
  - 为什么通用操作系统(Linux/Windows)选择死锁忽略策略

---

## 🔮 第2章展望

**下一章（第3章）**：
- 内存管理(Memory Management)：从内存使用与分段到分页与虚拟内存
- 涉及段表(Segment Table)、页表(Page Table)、TLB 等核心机制
- 操作系统如何将有限的物理内存扩展为巨大的虚拟地址空间

---

> 🔗 **返回根目录**：[Course Notes Root](../README.md)
