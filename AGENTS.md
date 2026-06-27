<!-- From: /Users/qinwenyan/Desktop/cpp/Operating_System_note/AGENTS.md -->
# AGENTS.md — 操作系统学习笔记项目

> 本文件面向 AI 编程助手。阅读前默认你对本项目一无所知。文件内容基于仓库实际结构和 `README.md` 编写，未做假设性推断。

---

## 项目概述

本项目是一个**中文 Markdown 学习笔记仓库**，主题为中国哈尔滨工业大学李治军老师在 B 站讲授的《操作系统》课程。仓库以章节为单位整理课程知识点，目标是形成覆盖操作系统四大核心模块的体系化笔记：

1. 操作系统基础
2. 进程与线程
3. 内存管理
4. 设备驱动与文件系统

截至目前，仓库已完成的章节为：

- **第 1 章：操作系统基础**（`Chapter01_Operating_System_Fundamentals/`）—— 7 个小节，完整覆盖
- **第 2 章：进程与线程**（`Chapter02_Process_Thread/`）—— 12 个小节，完整覆盖
- **第 3 章：内存管理**（`Chapter03_Memory_Management /`）—— 6 个小节，已创建但尚未创建章节导航 `README.md`

第 4 章（设备驱动与文件系统）尚未创建，但根目录 `README.md` 的章节导航中已预留链接。

- 许可证：MIT License（`LICENSE`）
- 作者：Qinwen Yan
- 仓库语言：中文（文档、注释、提交信息等均使用中文）

---

## 仓库结构

```
Operating_System_note/
├── README.md                               # 项目主页与总览
├── LICENSE                                 # MIT 许可证
├── .gitignore                              # 仅忽略 macOS .DS_Store
├── AGENTS.md                               # 本文件：面向 AI 助手的项目说明
├── Chapter01_Operating_System_Fundamentals/  # 第1章：操作系统基础
│   ├── README.md                           # 第1章导航与重难点
│   ├── 1.1_What_is_OS.md
│   ├── 1.2_Open_the_OS.md
│   ├── 1.3_OS_Boot.md
│   ├── 1.4_OS_Interface.md
│   ├── 1.5_System_Call_Implementation.md
│   ├── 1.6_History_of_Operating_Systems.md
│   ├── 1.7_Recap_and_Plan.md
│   └── images/                             # 第1章配图（40 张 PNG）
├── Chapter02_Process_Thread/               # 第2章：进程与线程
│   ├── README.md                           # 第2章导航与重难点
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
│   └── images/                             # 第2章配图（82 张 PNG）
└── Chapter03_Memory_Management /           # 第3章：内存管理（目录名末尾带空格）
    ├── 3.1_Memory_And_Segmentation.md
    ├── 3.2_Memory_Partition_And_Paging.md
    ├── 3.3_Multilevel_Paging_And_TLB.md
    ├── 3.4_Segmentation_Paging.md
    ├── 3.5_Swap_In.md
    ├── 3.6_Swap_Out.md
    └── images/                             # 第3章配图（39 张 PNG）
```

### 需要注意的结构问题

1. **第 3 章目录名带空格**：实际目录名为 `Chapter03_Memory_Management `（末尾有一个空格），而根目录 `README.md` 中链接为 `./Chapter03_Memory_Management/README.md`。这会导致链接在某些文件系统或 GitHub 上无法直接跳转。
2. **第 3 章缺少 `README.md`**：目前只有小节文件，没有章节导航页。
3. **第 4 章尚未创建**：根目录 `README.md` 中第 4 章链接指向 `./Chapter04_Device_Drivers_and_File_Systems/README.md`，但该目录和文件均不存在。

---

## 技术栈与构建/运行方式

本项目是**纯静态文档仓库**，不依赖任何编程语言、构建工具或运行时环境。

- **无配置文件**：不存在 `pyproject.toml`、`package.json`、`Cargo.toml`、`pom.xml`、`go.mod` 等。
- **无构建流程**：没有 `Makefile`、CI/CD 脚本、Dockerfile 或静态站点生成器配置。
- **无运行时架构**：无需安装依赖、编译或部署。
- **推荐查看方式**：直接在支持 Markdown 的编辑器、GitHub/GitLab 页面或本地 Markdown 阅读器中浏览。

---

## 内容组织与模块划分

### 章节组织

每章独立为一个目录，目录内包含：

- 一个 `README.md` 作为该章导航页（第 1、2 章已存在；第 3 章目前未创建）。
- 若干按授课顺序命名的小节 Markdown 文件，格式为 `[章节号].[小节号]_[英文主题].md`。
- 一个 `images/` 目录，存放该小节引用的 PNG 配图，按数字编号（`1.png`、`2.png`…）。

### 小节内部结构

每个小节 Markdown 遵循统一的模板，例如：

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

同章各小节共享该章的 `images/` 目录，因此编号可能跨小节连续，但不保证严格按小节隔离。

---

## 文档写作规范

仓库的 `README.md` 中嵌入了一套完整的「笔记生成提示词」，是本项目实际遵循的写作规范。AI 助手在修改或新增笔记时应尽量遵守以下要点：

### 1. 内容顺序

- **严格保持原文顺序**，禁止逻辑重排。
- 每个独立段落、小标题、表格、公式、机制描述都需要覆盖。

### 2. 知识点格式

- 每个小节拆分为 **10–15 个知识点**。
- 知识点标题格式统一为：

  ```markdown
  ## ✅ 知识点{数字}: {知识点名称}
  ```

- 使用 HTML 锚点 `<a id="id{数字}"></a>` 实现目录跳转。

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
3. **链接准确性**：创建新章节或新小节后，应同步更新根目录 `README.md` 和对应章节目录页的导航链接，注意目录名大小写、单复数和空格（例如 `Chapter02_Process_Thread`、`Chapter03_Memory_Management`）。
4. **不要引入不必要的构建工具**：本项目是纯笔记仓库，新增依赖或构建脚本前请与维护者确认。
5. **忽略 `.DS_Store`**：`.gitignore` 已配置忽略 macOS 系统文件，提交前无需额外处理。
6. **修复已知结构问题**：建议统一目录命名（去除 `Chapter03_Memory_Management ` 末尾空格）、补全第 3 章 `README.md`，并确认根目录 `README.md` 中第 4 章链接状态。

---

## 安全与版权

- 本项目代码/文档采用 MIT License 开源。
- 课程原视频版权归哈工大李治军老师及 B 站相关方所有，仓库仅整理个人学习笔记，引用课程链接时请保留原始出处。

---

> 本文件最后基于仓库当前实际内容整理。如果项目后续引入构建工具、测试或新的章节结构，请同步更新本 `AGENTS.md`。
