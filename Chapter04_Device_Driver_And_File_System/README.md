# 第4章 设备驱动与文件系统 (Device Drivers and File Systems)

> **章节定位**：从字符设备（显示器、键盘）的驱动机制出发，经过生磁盘访问与磁盘调度，逐层构建文件、目录与文件系统抽象，最终串联起从 `open()` 到磁盘 I/O 的完整代码链路。

---

## 📋 章节导航与重难点

### 4.1 [I/O 与显示器](./4.1_IO_And_Display.md)
- **核心**：
  - 操作系统四大资源：CPU、内存、磁盘、终端设备
  - CPU 与外设交互四步：写控制器寄存器 → 控制器驱动外设 → 中断通知 → 数据读入内存
  - 文件视图(File View)：统一设备接口 `open/read/write/close`
  - `printf` → `write(fd=1)` → `sys_write` 的调用链
  - `filp[]` 文件描述符数组、`file_table[]` 与 `inode` 的关系
  - 字符设备分发：`crw_table` 函数指针跳转表
  - `tty_write` 写队列缓冲与 `con_write` 显存写入
  - CGA 文本模式显存 `0xB8000` 与字符属性字节
- **难点**：
  - `fd` 只是索引，`current->filp[fd]` 才指向真实 `file` 结构
  - `dup(0)` 复制的是文件描述符而非文件内容
  - `crw_table` 通过主设备号实现设备多态分发
  - `tty_struct.write` 函数指针指向 `con_write` 的绑定时机
  - 显存中每个字符占 2 字节（字符 + 属性），`pos += 2` 的原因

### 4.2 [键盘](./4.2_Keyboard.md)
- **核心**：
  - 键盘中断驱动机制：0x21 号中断、`keyboard_interrupt`
  - 扫描码(Scan Code)与 ASCII 码的区分
  - `key_table` 函数指针数组实现散转
  - `key_map`/`shift_map`/`alt_map` 键盘映射表
  - `do_self` 完成扫描码到 ASCII 的转换与修饰键处理
  - `put_queue` 将字符放入 `read_q`
  - `copy_to_cooked` 加工原始输入并实现回显
  - 三级队列：`read_q` → `secondary` + `write_q`
- **难点**：
  - 扫描码 ≠ ASCII，需两级映射才能显示字符
  - `mode` 字节记录 Shift/Alt/Ctrl/Caps 状态
  - 回显(Echo)可控：`L_ECHO(tty)` 决定是否在屏幕显示输入
  - 键盘中断处理程序只做最小工作，复杂处理交给 `copy_to_cooked`
  - 三级队列如何解耦硬件输入、屏幕输出与程序读取

### 4.3 [生磁盘的使用](./4.3_Raw_Disk_Usage.md)
- **核心**：
  - 磁盘物理结构：磁道(Track)、扇区(Sector)、柱面(Cylinder)、磁头(Head)
  - 磁盘访问时间 = 控制器时间 + 寻道时间 + 旋转时间 + 传输时间
  - CHS（柱面/磁头/扇区）直接访问与端口编程 `hd_out`
  - 第一层抽象：盘块号(Block) → CHS，LBA 线性扇区号公式
  - 盘块(Block)设计：空间利用率与读写速度的权衡
  - 第二层抽象：多进程请求队列与磁盘调度
  - 磁盘调度算法：FCFS、SSTF、SCAN、C-SCAN
  - Linux 0.11 电梯算法：`make_request`、`add_request`、`IN_ORDER`
- **难点**：
  - 寻道时间是磁盘 I/O 的主要矛盾
  - LBA 与 CHS 的相互转换公式
  - 盘块大小越大速度越快但碎片越多
  - SSTF 的饥饿问题与 SCAN 的两端不对称问题
  - C-SCAN 复位阶段不处理请求的本质
  - `cli/sti` 关中断保护请求队列临界区

### 4.4 [从生磁盘到文件](./4.4_From_Raw_Disk_To_File.md)
- **核心**：
  - 文件是磁盘使用的第三层抽象
  - 文件本质：字符流(Character Stream)到盘块集合(Block Collection)的映射
  - 连续结构(Contiguous Allocation)：快但扩展困难
  - 链式结构(Linked Allocation)：易扩展但随机访问慢
  - 索引结构(Indexed Allocation)：折中方案
  - UNIX inode 多级索引：直接指针、一阶/二阶/三阶间接指针
  - 不同场景下文件存储结构的选择
- **难点**：
  - 三种文件实现结构的优缺点对比
  - inode 多级索引最大文件大小的计算
  - 直接指针与间接指针的访问开销差异
  - 为什么通用操作系统选择索引结构

### 4.5 [文件使用磁盘的实现](./4.5_Files_Implementation.md)
- **核心**：
  - `write(fd)` → `sys_write()` → `file_write()` 的调用链
  - `file_write` 三步：确定字符区间、找到盘块号、拷贝数据
  - `create_block` / `_bmap`：逻辑块号到物理盘块号的映射
  - `m_inode`（内存 inode）与 `d_inode`（磁盘 inode）的区别
  - 普通文件与设备文件对 `i_zone` 的复用差异
  - 文件视图统一磁盘与设备两条通路
  - `proc` 文件系统：从内核数据结构实时生成数据
- **难点**：
  - `_bmap` 中直接块、一重间接块、二重间接块的分级查找
  - `bh->b_dirt = 1` 标记脏页与延迟写回机制
  - 设备文件用 `i_zone[0]` 存设备号而非数据块指针
  - `proc` 文件没有磁盘块，`f_pos` 控制读取进度
  - inode 的 `i_mode` 如何实现"多态"数据通路选择

### 4.6 [目录与文件系统](./4.6_File_System.md)
- **核心**：
  - 文件系统是对磁盘的第四层抽象
  - 目录树解决文件集合划分、命名冲突与权限管理问题
  - 目录本质：特殊文件，内容是目录项数组
  - 目录项格式：`⟨文件名, FCB 地址⟩`
  - 树状目录解析：从根目录 inode 出发逐层查找
  - 磁盘六大区域：引导块、超级块、inode 位图、盘块位图、inode 数组、数据区
  - 自举信息：引导块与超级块的作用
- **难点**：
  - 目录项为何只存文件名 + FCB 地址，而非完整 FCB
  - 目录也是文件：FCB 的 `i_zone` 指向目录数据块
  - 根目录 inode 固定放在 inode 数组第一个位置
  - 位图如何用位向量管理 inode 与盘块的空闲状态
  - 超级块 `mount` 时读取以定位根目录

### 4.7 [目录解析代码实现](./4.7_Directory_Resolution.md)
- **核心**：
  - 目录解析完整调用链：`sys_open` → `open_namei` → `dir_namei` → `get_dir`
  - `get_dir`：从根目录或当前目录出发逐层解析路径
  - `find_entry`：在目录数据块中按文件名匹配目录项
  - `iget`：根据设备号和 inode 号读取 inode
  - `read_inode`：通过磁盘布局公式计算 inode 所在块
  - `mount_root`：系统启动时挂载根目录
  - 跨块目录遍历：`bmap` 负责逻辑块号到物理块号转换
- **难点**：
  - `get_dir` 返回的是最后一层目录的 inode，不是文件本身
  - `ROOT_INO = 1` 根目录 inode 号固定
  - inode 块号计算：`2 + s_imap_blocks + s_zmap_blocks + (i_num-1)/INODES_PER_BLOCK`
  - `find_entry` 跨多个数据块时的遍历逻辑
  - 目录项结构 `dir_entry` 中 `inode` 号与 `name[14]` 的关系

---

## 🔮 第4章展望

**课程总结**：
- 第1章奠定基础：操作系统接口与启动
- 第2章管理 CPU：进程、线程、调度与同步
- 第3章管理内存：分段、分页、虚拟内存与置换
- 第4章管理外设与磁盘：设备驱动、文件系统、目录解析
- 操作系统核心目标：管理硬件资源，为用户提供简洁、统一、安全的抽象接口

---

> 🔗 **返回根目录**：[Course Notes Root](../README.md)
