# langos/meta 说明（位于 blueprints）

`langos/meta` 过去用于存放基础规范和元信息，供人类和 AI 在使用业务文档库前了解规则。现迁至文档仓，便于与代码仓分离；运行时产出后独立可用，docs 主要承载蓝图与背景。

它回答的问题：

- 这套知识库/文档库（以及“自然语言编程操作系统”）的愿景、原则与分层是什么？
- 文档应该如何命名、编写、放置（在 docs 仓内的规范 vs 运行时内核的规范分别在哪里）？
- AI 在这里工作时，有哪些通用的行为约定与确认要求？

简言之：**业务知识库目录由你指定，本文件描述 meta 层的定位与导航**。

## 快速导航

- 顶层目录地图与最小使用路径：见 `repos/langos.md`。  
- AI 初始化必读（运行时最小集，在代码仓）：  
  - `langos/runtime/guidelines.md`  
  - `langos/runtime/runtime.md`  
  - `langos/runtime/protocols/index.yaml`  
  - `langos/runtime/conventions.md`  
- docs 仓自身规范与迭代方式：  
  - `meta/README.md`：langos-docs 仓的定位与迭代规则；  
  - `meta/conventions.md`：langos-docs 仓内文档的结构/命名/分类规范。  
- 启动对话提示（代码仓）：`langos/runtime/startup.md`。  
- 业务资产位置：由你指定的业务文档目录/仓库（可以是本 docs 仓，或工作区的 `workspace/` 目录等），与代码仓分离；doc_root 缺省时可先按最小集工作，需读取业务/背景文档再指定。

## 目录内容（概念层）

- `meta/`（本仓）：langos-docs 仓自身的治理与规范：
  - `meta/README.md`：如何迭代 langos-docs 仓；
  - `meta/conventions.md`：本仓文档的结构/命名/分类约定。
- `blueprints/`：愿景/导航（本文件 `meta-intro.md`、`vision.md` 等），描述自然语言编程“OS”的顶层蓝图与分层定位。
- `repos/`：项目索引/简介（单文件，如 `repos/langos.md`），用于列出可以被治理/开发的目标 repo。
- `specs/`：内核及其他规格（可按仓建子目录，如 `specs/langos/`），为协议与规范变更提供结构化需求输入。
- `projects/`：跨仓或阶段性项目文档。
- `adr/`：架构决策记录。
- `domain/`：领域知识、术语表、数据模型等（例如 `domain/glossary.md`）。  
- `archive/`：归档目录。

与之对应的运行时层规范在代码仓 `langos/runtime/` 下，尤其是 `runtime/conventions.md`（运行时命名/落盘规范）和 `runtime/guidelines.md`（对话行为约束）。

## 面向对象：人类 + AI

- 对人类：  
  - 提供写文档的统一规范和目录约定（docs 侧看 `meta/conventions.md`；运行时侧看 `langos/runtime/conventions.md`），便于协作；  
  - 提供愿景与分层说明，帮助理解 langos 的边界与定位。
- 对 AI：  
  - 定义基础行为约束（如何提问、何时确认、如何命名/落盘），在初始化时应优先遵守；  
  - 明确“运行时最小集不依赖 docs 内容”，doc_root 仅在需要读取业务/背景文档时按需使用。

## 阅读顺序（建议）

1. 文档库 README（本仓根 README）/项目 overview。  
2. `meta/README.md`（docs 仓迭代规则）与 `meta/conventions.md`（docs 仓规范）。  
3. `blueprints/meta-intro.md`（本文件）和 `blueprints/vision.md`（愿景与分层）。  
4. 视需要阅读业务文档库索引（如 `repos/INDEX.md`）、运行时文档（在代码仓 `langos/runtime/*`）。

## 演进

- 核心文件（docs 侧）：`meta/README.md`、`meta/conventions.md`、`blueprints/vision.md`。  
- 运行时侧：`langos/runtime/conventions.md`、`langos/runtime/guidelines.md`、`langos/runtime/runtime.md`。  
- 常见命名/写法/流程说明：
  - 与 docs 目录/文档结构相关的，沉淀到 `meta/conventions.md`（单一规范源，其他文档引用即可，避免重复描述）；  
  - AI 行为与执行流程相关的，沉淀到 `langos/runtime/conventions.md` 或 `langos/runtime/guidelines.md`。  
- 重大变更可写 ADR（建议放在 docs 仓 `adr/`）并在 specs 中补充需求说明。
