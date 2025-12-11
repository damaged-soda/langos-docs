# langos/meta 说明（位于 blueprints）

`langos/meta` 过去用于存放基础规范和元信息，供人类和 AI 在使用业务文档库前了解规则。现迁至文档库，便于与代码仓分离；运行时产出后独立可用，docs 主要承载蓝图与背景。

它回答的问题：

- 这套知识库/文档库（以及“自然语言编程操作系统”）的愿景、原则与分层是什么？
- 文档应该如何命名、编写、放置？
- AI 在这里工作时，有哪些通用的行为约定与确认要求？

简言之：**业务知识库目录由你指定，本文件描述 meta 层的定位与导航**。

## 快速导航
- 顶层目录地图与最小使用路径：见 `repos/langos.md`。  
- AI 初始化必读：`langos/runtime/guidelines.md`、`langos/runtime/runtime.md`、`langos/runtime/protocols/index.yaml`（在代码仓，最小集即可运行）。  
- 启动对话提示：`langos/runtime/startup.md`（代码仓）。  
- 业务资产位置：由你指定的业务文档目录/仓库（例：本 docs 仓、或工作区的 `workspace/` 目录等），与代码仓分离；doc_root 缺省时可先按最小集工作，需读取业务/背景文档再指定。

## 目录内容
- `conventions`（代码仓 `langos/runtime/conventions.md`）：基础规范（命名/写作/AI 底线）。  
- `blueprints/`：愿景/导航（本文件 `meta-intro.md`、`vision.md`）。  
- `repos/`：项目索引/简介（单文件，如 `repos/langos.md`）。  
- `specs/`：内核及其他规格（可按仓建子目录，如 `specs/langos/`）。  
- 其他：`projects/`、`adr/`、`domain/`（术语与领域知识，如 `domain/glossary.md`）、`archive/`。***

## 面向对象：人类 + AI
- 对人类：提供写文档的统一规范和目录约定，便于协作。  
- 对 AI：定义基础行为约束（如何提问、何时确认、如何命名/落盘），在初始化时应优先遵守。  

## 阅读顺序（建议）
1. 文档库 README（本仓）/项目 overview。  
2. `meta-intro.md`（本文件）。  
3. `conventions.md`。  
4. 视需要阅读业务文档库索引（如 `repos/INDEX.md`）、运行时文档（在代码仓）。

## 演进
- 核心文件：`conventions.md`、`vision.md`。  
- 常见命名/写法/流程说明沉淀到 `conventions.md`；AI 易踩坑点可写在 `conventions.md` 或代码仓的 `runtime/guidelines.md`。  
- 重大变更可写 ADR（建议放在 docs 仓 `adr/`）。***
