# 术语说明（doc_root 与目录）

本文件解释 langos-docs 中常用的几个术语和目录名，帮助人类与 AI 共享同一套心智模型。  
关于 **具体命名/分类等硬规范**，请以 `meta/conventions.md` 为准，本文件只做概念解释。

- **doc_root**  
  - 含义：业务/背景文档的根目录，运行时通过配置中的 `doc_root` 定位。  
  - 位置：可以是本仓（`langos-docs/`），也可以是其他 workspace（例如 `workspace/docs/`），不要求一定是 Git 仓。  
  - 行为：启动时仅校验路径存在，不预读内容；需要读取业务/背景文档时再按需访问。

- **repos/**  
  - 含义：项目索引与简介目录，用来告诉人类/AI “有哪些目标仓库可以被治理/开发”。  
  - 内容：  
    - `repos/INDEX.md`：项目总索引；  
    - `repos/<name>.md`：单个仓库的概览与导航（如 `repos/langos.md`）。  
  - 说明：`repos/` 列的是“目标项目 repo”，而不是“当前所有 Git 仓”的列表。  
  - 具体规则：命名与职责的硬约束见 `meta/conventions.md`。

- **specs/**  
  - 含义：需求与设计规格目录，用于承载“改造/新增某能力”之前的结构化说明。  
  - 使用方式：  
    - 跨仓或通用规格：直接放在 `specs/`；  
    - 针对单个仓库的规格：放在 `specs/<repo>/`（如 `specs/langos/`）。  
  - 命名建议：`YYYYMM-short-name-spec.md`。  
  - 具体规范见 `meta/conventions.md`。

- **projects/**  
  - 含义：跨仓或阶段性项目文档目录。  
  - 常见文件：`overview.md` / `plan.md` / `milestones.md` / `notes.md` 等，描述目标、计划与进展。  
  - 典型用法：承载“一段时间内要完成的一揽子改动”，而不是长年不变的背景说明。

- **adr/**  
  - 含义：Architecture Decision Record，记录关键架构决策。  
  - 作用：说明决策的背景、备选方案、取舍与影响，便于长期追踪。  
  - 命名建议：`YYYYMMDD-short-name.md`。  
  - 详细格式建议见 `meta/conventions.md`。

- **domain/**  
  - 含义：领域知识与术语目录（本文件所在）。  
  - 典型内容：
    - `glossary.md`：术语表（本文件）；  
    - `data-model.md`：关键实体/关系/约束等。  
  - 用途：沉淀与业务域或治理域相关的“概念层”知识。

- **archive/**  
  - 含义：归档目录，存放已完成或过时的文档。  
  - 要求：归档时在文件头部注明：
    - 原始路径；  
    - 归档原因；  
    - 归档日期；  
    - 若有替代文档，给出链接。  

- **运行时最小集**  
  - 含义：langos 运行时在没有 doc_root 内容时也能工作的那部分文档/协议。  
  - 位置：`../langos/runtime/*`（包括 `guidelines.md`、`runtime.md`、`protocols/index.yaml` 等）。  
  - 特性：不依赖 doc_root 内容；需要业务/背景信息时才按配置的 doc_root 按需读取。

如需修改目录职责或命名规范，请先参考 `meta/README.md` 与 `meta/conventions.md`，必要时通过 specs/ 或 adr/ 引入规格与决策，再更新本文件中的术语说明。
