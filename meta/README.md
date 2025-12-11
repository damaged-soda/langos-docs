# langos-docs/meta 说明与迭代规则

本目录用于管理 **langos-docs 仓自身的治理与规范**，回答的问题是：

- 这个 docs 仓本身的定位是什么？
- 新文档/新分类/结构调整应该放哪、怎么改？
- 当 langos 的设计演进时，如何安全地迭代本仓的蓝图与规格？

运行时代码与协议在 `../langos/` 仓，本仓不参与运行时最小集，仅在**设计与治理阶段**被人类/AI 按需读取。

---

## 1. 仓库定位（langos-docs）

- 身份：langos 项目的 **蓝图 / 背景 / 规格 / 治理** 仓。
- 主要内容：
  - 愿景与分层蓝图：`blueprints/`（如 `meta-intro.md`、`vision.md`）。
  - 仓库索引与概览：`repos/`（如 `repos/langos.md`）。
  - 需求规格：`specs/`（含 `specs/langos/` 内核相关的规格）。
  - 项目文档：`projects/`。
  - 架构决策记录：`adr/`。
  - 领域知识与术语：`domain/`。
  - 历史归档：`archive/`。
- 非目标：
  - 本仓 **不是** 运行时依赖；langos 内核在没有本仓时也能按最小集运行。
  - 不放具体业务代码或运行时代码。

若只需要运行时能力，请直接使用 `../langos/` 仓；需要理解设计思路、规划迭代、补充规格/ADR 时，再按需读取本仓。

---

## 2. meta 目录职责

`meta/` 目录只做一件事：**治理 langos-docs 仓本身**。

当前包含：

- `meta/README.md`（本文件）：说明 langos-docs 的定位、meta 层的作用，以及“如何迭代本仓”。
- `meta/conventions.md`：langos-docs 仓内文档的结构/命名/写作规范（只约束 docs 仓，不覆盖 `langos/runtime/conventions.md`）。
- 后续如有需要，可在此补充：
  - 文档评审流程说明；
  - 权限/角色分工（谁可以改什么类型的文档）；
  - 文档模板说明等。

与之对应：

- **运行时规范**（AI 对话/协议行为）在 `langos/runtime/`，尤其是：
  - `langos/runtime/conventions.md`
  - `langos/runtime/guidelines.md`
  - `langos/runtime/runtime.md`
- **内核规格**（对协议/路由/解耦的需求）在 `langos-docs/specs/langos/`。

请区分：  
运行时怎么行为 → 看 `langos/` 仓；  
docs 仓怎么写 / 怎么分类 → 看 `meta/`。
- 文档结构/命名/分类的规范源 = `meta/conventions.md`（单一真相源）；其他文档如需提及规则，请引用该文件，避免多处维护或出现冲突表述。

---

## 3. 迭代 langos-docs 的基本流程

### 3.1 日常新增/修改文档

适用场景：补充说明、增加规格、写项目文档等，不涉及目录大调整。

推荐流程：

1. 确认目标类别  
   - 运行时内核规格 → `specs/langos/`  
   - 其他跨仓或通用规格 → `specs/` 下按主题/仓库建子目录  
   - 跨仓项目 → `projects/`  
   - 架构决策 → `adr/`  
   - 领域知识/术语 → `domain/`  
   - 过时/历史文档 → `archive/`
2. 参照 `meta/conventions.md` 确认文件命名规范。
3. 在对应目录创建/更新文档：
   - 开头写明背景和适用范围；
   - 必要时在相关 README 或索引里补一行链接（例如 `specs/README.md`、`repos/INDEX.md`）。
4. 提交时在 commit message 中简要说明变更目的。

如只是补充内容而未改变目录结构，一般不需要 ADR 或额外治理流程。

### 3.2 变更目录结构或分类规则

适用场景：调整目录划分、移动文档在不同目录间、修改分类约束或命名规则等。

推荐流程：

1. 写明变更意图与影响：
   - 为什么要调整目录/分类？
   - 影响到哪些已有文档/链接？
2. 选择承载形式：
   - 如果是一次性的结构调整，且影响有限，可直接在相关 README 中说明；
   - 如果是“长期生效的规则变更”，建议：
     - 在 `specs/langos/` 写一份规格（如 `YYYYMM-langos-docs-structure-spec.md`），或
     - 在 `adr/` 中写一条 ADR（记录决策和取舍）。
3. 落地变更：
   - 更新对应目录下的 README（至少包括仓根 README、受影响目录的 README、必要时更新 `domain/glossary.md` 和 `meta/conventions.md`）；
   - 视需要更新已有文档中的链接/引用。
4. 简要回顾：
   - 在 PR 或变更描述中，说明新旧结构的差异及迁移关系。

参考现有示例：  
`specs/langos/202512-langos-docs-classification-spec.md` 记录了上一轮分类与 repos/职责调整。

### 3.3 改变“langos ↔ docs 解耦”相关原则

适用场景：改变 langos 与 docs 的依赖关系、启动流程、doc_root 语义等核心设计。

因为这类变更直接影响运行时心智模型，建议：

1. 必须先在 `specs/langos/` 写或更新对应规格，例如更新：
   - `langos-runtime-docs-decoupling-spec.md`，或新增新的解耦/耦合规格。
2. 再根据规格调整：
   - `langos/runtime/*.md` 中的表述；
   - 本仓相关蓝图（如 `blueprints/meta-intro.md`、`blueprints/vision.md`）；
   - 必要时更新本文件（`meta/README.md`）和 `meta/conventions.md`。
3. 如变更具备明显“决策”属性，也可以额外写一条 ADR 方便长期追踪。

---

## 4. 与其他目录的关系

简要映射如下（详细术语说明见 `domain/glossary.md`）：

- `blueprints/`：描述愿景、分层、整体治理思路；不会约束具体目录细节。
- `repos/`：列出项目索引与单仓概览（例如 `repos/langos.md`），不放治理/规格正文。
- `specs/`：存放需求与设计规格，是协议与规范变更的上游输入。
- `projects/`：描述跨仓或阶段性项目的目标、计划与进展。
- `adr/`：记录关键架构决策的上下文与取舍。
- `domain/`：沉淀领域知识与术语，解释 doc_root / repos / specs 等概念。
- `archive/`：存放已过时或完成的文档，避免干扰当前信息。
- `meta/`（本目录）：约束和解释 **langos-docs 仓本身** 的使用与演进。

当你不确定某份新文档应该放哪时，建议：

1. 先看 `meta/conventions.md` 和 `domain/glossary.md` 的目录责任说明；
2. 再结合当前变更意图，选择最贴近的目录；
3. 如果现有结构真不适合，再按 3.2 所述流程发起结构调整。

---

## 5. 面向 AI 的使用提示（简版）

- 初始化时：
  - 若需要理解这套文档库本身的规则，先阅读 `meta/README.md` 与 `meta/conventions.md`；
  - 若需要理解 langos 内核的愿景与边界，再阅读 `blueprints/meta-intro.md`、`blueprints/vision.md`。
- 处理“新增/调整规范、目录、规格分类”类请求时：
  - 优先遵守 `meta/conventions.md`；
  - 变更目录/规则前，先向人类复述你的理解与计划，并提示是否需要规格或 ADR 支撑。
- 不要假设运行时依赖本仓内容：
  - 如遇到与运行时行为相关的问题，先以 `langos/` 仓为依据；
  - 本仓内容仅作为设计/治理参考。
