# 第3章 内存管理 (Memory Management)

> **章节定位**：围绕操作系统如何为进程分配、翻译和管理内存展开，从程序重定位到分段分页机制，再到虚拟内存的请求调页与页面置换，为理解现代操作系统的内存子系统奠定基础。

---

## 📋 章节导航与重难点

### 3.1 [内存使用与分段](./3.1_Memory_And_Segmentation.md)
- **核心**：
  - 冯·诺依曼架构(Von Neumann Architecture)下程序执行的基本循环
  - 重定位(Relocation)三时机：编译时、载入时、运行时(Runtime Relocation)
  - 运行时重定位基地址(Base)机制：`物理地址 = base + offset`
  - 内存管理单元(MMU)与 `base`/`limit` 寄存器
  - 分段(Segmentation)机制：代码段、数据段、栈段、堆段
  - 段表(Segment Table)与 `<段号:偏移>` 地址翻译
  - x86 分段实现：GDT(Global Descriptor Table) 与 LDT(Local Descriptor Table)
- **难点**：
  - 编译时/载入时/运行时重定位的区别与适用场景
  - 段表查询多了一次间接访问，容易混淆段号与段基址
  - GDT 与 LDT 的层级关系：LDT 描述符为何存放在 GDT 中
  - 段选择子(Selector)、描述符(Descriptor)与段基址的关系

### 3.2 [内存分区与分页](./3.2_Memory_Partition_And_Paging.md)
- **核心**：
  - 固定分区(Fixed Partition)与内部碎片(Internal Fragmentation)
  - 可变分区(Variable Partition)与空闲/已分配分区表
  - 首次适配(First Fit)、最佳适配(Best Fit)、最差适配(Worst Fit)
  - 外部碎片(External Fragmentation)与内存紧缩(Memory Compaction)
  - 分页(Paging)机制：页(Page)与页框(Page Frame)
  - 页表(Page Table)数据结构
  - 地址翻译(Address Translation)：`Page#` + `Offset` → 物理地址
- **难点**：
  - 内部碎片与外部碎片的区分
  - 三种适配算法的比较与最佳适配的碎片问题
  - 内存紧缩的时间开销为何难以接受
  - 页框号(Frame Number)与页号(Page Number)的概念区分
  - 地址翻译中的位运算拆分（高 N 位页号 + 低 12 位偏移）

### 3.3 [多级页表和快表](./3.3_Multilevel_Paging_And_TLB.md)
- **核心**：
  - 页表大小的核心矛盾：页小则页表大
  - 32 位系统 + 4KB 页面 = 4MB 单级页表/进程
  - 多级页表(Multi-level Page Table)结构：页目录 + 页表
  - 10 + 10 + 12 位地址划分
  - TLB(Translation Lookaside Buffer)快表机制
  - 相联存储(Associative Memory)与 TLB 命中/失效
  - 有效访问时间 EAT(Effective Access Time)公式
  - 局部性原理(Locality)：空间局部性与时间局部性
- **难点**：
  - 为什么页号必须连续，否则查找开销剧增
  - 多级页表如何用时间换空间
  - TLB 命中与未命中的访存次数差异
  - EAT 公式的代入计算
  - TLB 容量很小却有效的原因：程序访问的局部性

### 3.4 [段页结合的实际内存管理](./3.4_Segmentation_Paging.md)
- **核心**：
  - 段页结合(Segmentation & Paging)动机：段面向用户、页面向硬件
  - 两级地址转换：逻辑地址 → 段表 → 虚拟地址 → 页表 → 物理地址
  - Linux 0.11 `fork()` 内存分配入口 `copy_mem`
  - `copy_page_tables` 建立子进程页表
  - 页目录项地址计算：`(addr >> 20) & 0xffc`
  - `get_free_page` 与页表项建立
  - 写时复制(Copy-On-Write, COW)机制
- **难点**：
  - 段表和页表同时存在时的两次地址转换
  - `copy_mem` 中每个进程 64M 虚拟地址空间的设计
  - `copy_page_tables` 中位运算与页目录项/页表项的提取
  - COW 中父子页表项都设为只读的原因
  - 缺页异常触发 COW 后分配新物理页的完整流程

### 3.5 [请求调页内存换入](./3.5_Swap_In.md)
- **核心**：
  - 虚拟内存(Virtual Memory)的用户视角：4GB 连续地址空间
  - 请求调页(Demand Paging)与惰性加载(Lazy Loading)
  - 换入(Swap In)与换出(Swap Out)的方向
  - 请求调页 vs 请求调段(Demand Segmentation)
  - 缺页中断(Page Fault)：14 号中断
  - `page.s` 汇编入口：保存现场、读取 CR2、分发处理
  - `do_no_page` 缺页处理函数
  - `put_page` 建立虚拟地址到物理页的映射
- **难点**：
  - 为何请求调页粒度更细、效率更高
  - 页错误中断如何让 PC 原地不动，重新执行触发指令
  - `page.s` 中错误码 P 位判断：P=0 缺页、P=1 写保护
  - `do_no_page` 中代码/数据段与堆栈段的区分处理
  - `put_page` 中多级页表索引与页表项标志位 `| 7` 的含义

### 3.6 [内存换出](./3.6_Swap_Out.md)
- **核心**：
  - 换出必要性：`get_free_page()` 失败时淘汰页面
  - FIFO(First-In First-Out)页面置换算法与 Belady 异常
  - MIN(Optimal)最优页面置换算法
  - LRU(Least Recently Used)最近最少使用算法
  - LRU 准确实现：时间戳法(Time Stamp)与页码栈法(Page Stack)
  - Clock 算法(SCR, Second Chance Replacement)与引用位(Reference Bit)
  - 双指针时钟：快指针清 R、慢指针淘汰
  - 页框分配(Frame Allocation)与颠簸(Thrashing)现象
- **难点**：
  - FIFO 的 Belady 异常：页框增多缺页反而增加
  - MIN 理论最优但不可实现的原因
  - LRU 准确实现代价高：时间戳与栈操作开销
  - Clock 算法在缺页很少时退化为 FIFO 的问题
  - 双指针时钟如何平衡"忘记历史"与"近期未使用"
  - 颠簸现象的恶性循环机制

---

## 🔮 第3章展望

**下一章（第4章）**：
- 转向设备驱动与文件系统(Device Drivers and File Systems)
- 涉及外设管理、I/O 接口、磁盘结构与文件系统实现
- 内存管理中的缓冲、缓存与磁盘交换将进一步衔接

---

> 🔗 **返回根目录**：[Course Notes Root](../README.md)
