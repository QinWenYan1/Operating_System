<!-- From: /Users/qinwenyan/Desktop/cpp/Operating_System_note/AGENTS.md -->
# AGENTS.md — 操作系统学习笔记项目

> 本文件面向 AI 编程助手。阅读前默认你对本项目一无所知。以下内容基于对仓库实际文件、目录和 `README.md` 的直接探查整理，未做推断性假设。

---

## 项目概述

本项目是一个**纯中文 Markdown 学习笔记仓库**，主题为中国哈尔滨工业大学李治军老师在 B 站讲授的《操作系统》课程。仓库以章节为单位整理课程知识点，目标是覆盖操作系统四大核心模块：

1. 操作系统基础
2. 进程与线程
3. 内存管理
4. 设备驱动与文件系统

当前仓库已完成的内容（按实际文件统计）：

- **第 1 章：操作系统基础**（`Chapter01_Operating_System_Fundamentals/`）—— 7 个小节 + 章节导航 `README.md` + 40 张配图
- **第 2 章：进程与线程**（`Chapter02_Process_Thread/`）—— 12 个小节 + 章节导航 `README.md` + 82 张配图
- **第 3 章：内存管理**（`Chapter03_Memory_Management /`）—— 6 个小节 + 章节导航 `README.md` + 48 张配图
- **第 4 章：设备驱动与文件系统**（`Chapter04_Device_Driver_And_File_System/`）—— 7 个小节 + 46 张配图，**缺少章节导航 `README.md`**

- 许可证：MIT License（`LICENSE`）
- 作者：Qinwen Yan（Copyright 2026）
- 仓库语言：中文（文档、注释、链接标题、提交信息等均使用中文）

---

## 仓库结构

```
Operating_System_note/
├── README.md                                   # 项目主页、章节导航与笔记生成提示词
├── LICENSE                                     # MIT 许可证
├── .gitignore                                  # 仅忽略 macOS .DS_Store
├── AGENTS.md                                   # 本文件：面向 AI 助手的项目说明
├── Chapter01_Operating_System_Fundamentals/    # 第1章：操作系统基础
│   ├── README.md                               # 第1章导航与重难点
│   ├── 1.1_What_is_OS.md
│   ├── 1.2_Open_the_OS.md
│   ├── 1.3_OS_Boot.md
│   ├── 1.4_OS_Interface.md
│   ├── 1.5_System_Call_Implementation.md
│   ├── 1.6_History_of_Operating_Systems.md
│   ├── 1.7_Recap_and_Plan.md
│   └── images/                                 # 第1章配图（40 张 PNG）
├── Chapter02_Process_Thread/                   # 第2章：进程与线程
│   ├── README.md                               # 第2章导航与重难点
│   ├── 2.1_Intuitive_Ideas_of_CPU_Management.md
│   ├── 2.2_Multiple_Processes.md
│   ├── 2.3_User_Level_Threads.md
│   ├── 2.4_Kernel_Threads.md
│   ├── 2.5_Kernel_Thread_Implementation.md
│   ├── 2.6_The_Tree_Of_Os.md
│   ├── 2.7_CPU_Scheduling.md
│   ├── 2.8_A_Practical_Schedule_Function.md
│   ├── 2.9_Processes_Synchronization_And_Semaphore.md
│   ├── 2.10_Critical_Section.md
│   ├── 2.11_Coding_Semaphore.md
│   ├── 2.12_Deadlock_Handling.md
│   └── images/                                 # 第2章配图（82 张 PNG）
├── Chapter03_Memory_Management /               # 第3章：内存管理（目录名末尾含一个空格）
│   ├── README.md                               # 第3章导航与重难点
│   ├── 3.1_Memory_And_Segmentation.md
│   ├── 3.2_Memory_Partition_And_Paging.md
│   ├── 3.3_Multilevel_Paging_And_TLB.md
│   ├── 3.4_Segmentation_Paging.md
│   ├── 3.5_Swap_In.md
│   ├── 3.6_Swap_Out.md
│   └── images/                                 # 第3章配图（48 张 PNG）
└── Chapter04_Device_Driver_And_File_System/    # 第4章：设备驱动与文件系统
    ├── 4.1_IO_And_Display.md
    ├── 4.2_Keyboard.md
    ├── 4.3_Raw_Disk_Usage.md
    ├── 4.4_From_Raw_Disk_To_File.md
    ├── 4.5_Files_Implementation.md
    ├── 4.6_File_System.md
    ├── 4.7_Directory_Resolution.md
    └── images/                                 # 第4章配图（46 张 PNG）
```

### 当前已知的结构/链接问题

1. **第 3 章目录名带尾部空格**：实际目录名为 `Chapter03_Memory_Management `（末尾有一个空格），而根目录 `README.md` 中的链接写作 `./Chapter03_Memory_Management/README.md`。该链接在部分文件系统或 GitHub 上无法直接跳转。
2. **第 4 章缺少章节导航 `README.md`**：目前只有小节文件和配图，没有统一入口页。
3. **根目录 `README.md` 中第 4 章链接不匹配**：链接指向 `./Chapter04_Device_Drivers_and_File_Systems/README.md`（使用了复数 `Drivers`/`Systems`），而实际目录名为 `Chapter04_Device_Driver_And_File_System/`，且该目录下尚无 `README.md`。
4. **工作区存在未提交修改**：探查时 `git status --short` 显示 `Chapter04_Device_Driver_And_File_System/4.7_Directory_Resolution.md` 处于已修改（`M`）未提交状态。

---

## 技术栈与构建/运行方式

本项目是**纯静态文档仓库**，不依赖任何编程语言、构建工具或运行时环境。

- **无配置文件**：仓库中不存在 `pyproject.toml`、`package.json`、`Cargo.toml`、`pom.xml`、`go.mod`、`Makefile`、`CMakeLists.txt`、`Dockerfile`、CI/CD 配置文件等。
- **无构建流程**：没有预编译、打包、站点生成或部署脚本。
- **无运行时架构**：无需安装依赖、编译或启动服务。
- **推荐查看方式**：直接在支持 Markdown 的编辑器、GitHub/GitLab 页面或本地 Markdown 阅读器中浏览。

因此，**没有可执行的构建命令或测试命令**。任何新增依赖、构建脚本或 CI/CD 配置前都应与维护者确认。

---

## 内容组织与模块划分

### 章节组织

每章独立为一个目录，目录内包含：

- 一个 `README.md` 作为该章导航页（第 1、2、3 章已存在；第 4 章目前未创建）。
- 若干按授课顺序命名的小节 Markdown 文件，格式为 `[章节号].[小节号]_[英文主题].md`。
- 一个 `images/` 目录，存放该章引用的 PNG 配图，按数字编号（`1.png`、`2.png`…）。同章各小节共享该 `images/` 目录。

### 小节内部结构

每个小节 Markdown 遵循统一模板，典型结构如下：

```markdown
# 📘 [章节编号] [章节标题] ([英文原文标题])

> 来源说明：哈工大操作系统 [课程信息]

## 🧠 核心概念总览（严格按原文顺序）
- [*知识点1: 知识点名称*](#id1)
- [*知识点2: 知识点名称*](#id2)
...

<a id="id1"></a>
## ✅ 知识点1: [知识点名称]
...

## 🔑 核心要点总结
...

## 📌 考试速记版
...
```

### 图片引用约定

图片使用相对路径引用：

```markdown
![alt text](images/1.png)
```

同章各小节共享该章的 `images/` 目录，编号跨小节连续，不保证按小节隔离。

---

## 文档写作规范

仓库根目录 `README.md` 中嵌入了一套完整的「笔记生成提示词」，是本项目实际遵循的写作规范。AI 助手在修改或新增笔记时应遵守以下要点：

### 1. 内容顺序

- **严格保持原文顺序**，禁止逻辑重排。
- 每个独立段落、小标题、表格、公式、机制描述都需要覆盖。

### 2. 知识点格式

- 每个小节拆分为 **10–15 个知识点**。
- 知识点标题格式统一为：

  ```markdown
  ## ✅ 知识点{数字}: {知识点名称}
  ```

- 使用 HTML 锚点 `<a id="id{数字}"></a>` 实现目录跳转，锚点编号按知识点出现顺序递增。

### 3. 术语处理

- 关键术语必须提供中英文对照，格式为：
  - `中文术语(English Term)`，例如：`操作系统(Operating System)`
  - 或 `English Term(中文解释)`

### 4. 重点强调

- 核心概念、关键警告、重要区别使用 **加粗**。
- 术语、函数名、寄存器名、地址使用行内代码 `` ` ``。
- 公式使用 LaTeX：`$行内公式$` 或 `$$块级公式$$`。

### 5. 表情符号使用

| 符号 | 用途 |
|------|------|
| 📘 | 章节标识 |
| 🧠 | 核心概念总览 |
| ✅ | 知识点块 |
| ⚠️ | 警告注意 |
| 💡 | 技巧提示 |
| 🔄 | 知识关联 |
| 📋 | 术语提醒 |
| 🔧 | 优化处理 |
| 🔑 | 核心要点总结 |
| 📌 | 考试速记版 |

### 6. 状态转换描述

所有状态转换图或状态机描述**必须使用列表形式**，禁止使用表格。

### 7. 代码块

代码示例保持教材原样，使用合适的语言标签，例如：

```markdown
```c
// C 代码示例
```
```

---

## 测试、部署与 CI/CD

- **无测试**：本项目没有单元测试、集成测试或任何自动化测试脚本。
- **无部署流程**：不需要构建、打包或发布。
- **无 CI/CD**：没有 `.github/workflows`、`.gitlab-ci.yml` 或其他持续集成配置。
- 修改内容后，通常只需直接提交 Markdown 文件和对应图片即可。

---

## 开发注意事项

1. **保持中文为主**：所有新增文档、注释、链接标题均使用中文；术语保留英文原文对照。
2. **图片统一管理**：配图放在对应章节的 `images/` 目录下，避免直接引用外部图床链接。
3. **链接准确性**：创建新章节或新小节后，应同步更新根目录 `README.md` 和对应章节目录页的导航链接，注意目录名大小写、单复数以及尾部空格（例如 `Chapter02_Process_Thread`、`Chapter03_Memory_Management`）。
4. **不要引入不必要的构建工具**：本项目是纯笔记仓库，新增依赖或构建脚本前请与维护者确认。
5. **忽略 `.DS_Store`**：`.gitignore` 已配置忽略 macOS 系统文件，提交前无需额外处理。
6. **建议修复的结构性问题**：
   - 统一目录命名，去除 `Chapter03_Memory_Management ` 末尾空格；
   - 补全第 4 章 `README.md` 章节导航页；
   - 修正根目录 `README.md` 中第 3、4 章的链接，使其与实际目录名一致。

---

## 安全与版权

- 本项目代码/文档采用 MIT License 开源。
- 课程原视频版权归哈工大李治军老师及 B 站相关方所有，仓库仅整理个人学习笔记，引用课程链接时请保留原始出处。

---

> 本文件基于仓库实际内容整理。如果项目后续引入构建工具、测试、新的章节结构或修复了已知目录/链接问题，请同步更新本 `AGENTS.md`。
