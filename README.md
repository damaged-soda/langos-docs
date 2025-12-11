# langos-docs

langos 项目的高层文档仓库，用于存放 **愿景蓝图、设计背景、需求规格、项目文档、ADR 与领域知识**。

- 运行时内核与协议代码在 `../langos/`，可以在没有本仓的情况下按最小集运行。
- 本仓仅在需要 **理解设计思路 / 规划迭代 / 记录决策与规格** 时由人类或 AI 按需读取。

当你只想“用” langos（执行协议、处理业务需求）时，直接阅读并使用 `../langos/` 即可；  
当你想“改” langos 或治理项目时，再回到本仓。

---

## 1. 面向谁、在什么场景使用

本仓主要服务两类场景：

- **治理与设计者**  
  - 规划或调整 langos 的整体愿景、分层、协议体系；
  - 为运行时新增/修改协议前，先写需求规格与 ADR；
  - 讨论文档结构、分类规则与演进方式。

- **高级使用者 / 维护者**  
  - 需要深入理解 langos 的定位、边界与使用方式；
  - 希望按规范编写 specs、projects、adr、domain 文档；
  - 需要查阅历史决策与归档内容。

普通业务开发场景下，优先以 `../langos/` 仓为主；  
需要背景与治理信息时，再回到本仓查阅。

---

## 2. 推荐阅读顺序（首次接触）

若你是第一次维护或治理 langos，建议按下面顺序阅读：

1. **仓库定位与治理规则**
   - `meta/README.md`：本仓的定位、如何迭代 langos-docs。
   - `meta/conventions.md`：本仓文档的结构、命名与分类规范。

2. **愿景与分层蓝图**
   - `blueprints/meta-intro.md`：meta 层说明与整体导航。
   - `blueprints/vision.md`：自然语言编程“OS”的愿景与分层定位。

3. **项目索引与内核概览**
   - `repos/INDEX.md`：项目/仓库索引。
   - `repos/langos.md`：langos 内核的概览与导航（指向 `../langos/` 和本仓相关文档）。

4. **规格与决策记录（按需）**
   - `specs/README.md`：规格目录说明。
   - `specs/langos/`：langos 内核相关规格，例如：
     - `202512-langos-docs-classification-spec.md`：本仓分类与 repos/职责调整说明。
     - `langos-runtime-docs-decoupling-spec.md`：运行时与文档解耦需求规格。
   - `adr/`：关键架构决策记录（如已存在）。

5. **术语与目录约束**
   - `domain/glossary.md`：doc_root、repos、specs、projects、adr、domain、archive 等术语说明。

之后，可根据具体任务再深入 `projects/`、`archive/` 等目录。

---

## 3. 目录结构与职责概览

本仓采用按职责划分的目录结构；各目录的详细说明见各自的 `README.md`，这里只做简要概览：

- `meta/`  
  - **仓库治理与规范**。  
  - `meta/README.md`：如何迭代 langos-docs 仓。  
  - `meta/conventions.md`：本仓文档的结构/命名/分类约定。

- `blueprints/`  
  - **愿景与分层蓝图**。  
  - 存放顶层 meta 文档，如 `meta-intro.md`、`vision.md`；  
  - 不放具体仓库说明或需求规格。

- `repos/`  
  - **项目索引与单仓 overview**。  
  - `repos/INDEX.md`：项目列表与简要说明；  
  - `repos/<name>.md`：单个仓库的概览与导航（例如 `repos/langos.md`）。

- `specs/`  
  - **需求与设计规格**。  
  - 通用或跨仓规格直接放在 `specs/`；  
  - 针对某个仓库的规格放在 `specs/<repo>/`（例如 `specs/langos/`）。

- `projects/`  
  - **跨仓或阶段性项目文档**。  
  - 建议使用 `YYYYMM-project-name/` 目录，内含 `overview.md` / `plan.md` / `notes.md` 等。

- `adr/`  
  - **架构决策记录（Architecture Decision Record）**。  
  - 命名建议 `YYYYMMDD-short-name.md`，每条 ADR 聚焦一个决策主题。

- `domain/`  
  - **领域知识与术语**。  
  - 包含 `glossary.md`、数据模型等；  
  - 解释 doc_root / repos / specs 等概念。

- `archive/`  
  - **历史归档**。  
  - 存放已完成或过时的文档；归档时需在文件开头注明来源路径、归档原因、日期及替代路径（如有）。

各目录的使用细则与约束以 `meta/conventions.md` 与目录内 README 为准。

---

## 4. 与 langos 代码仓的关系

- 运行时内核与协议目录在 `../langos/`：  
  - `langos/runtime/startup.md`：启动引导与模式选择；  
  - `langos/runtime/guidelines.md`：对话流程与行为约束；  
  - `langos/runtime/runtime.md`：运行时流程与确认闸门；  
  - `langos/runtime/protocols/`：协议与索引；  
  - `langos/runtime/conventions.md`：运行时命名与落盘规范。

- 本仓与代码仓的关系：
  - 本仓提供蓝图、规格与决策记录，为 **设计与治理阶段** 提供上下文；  
  - 代码仓提供可执行的运行时内核与协议，为 **执行阶段** 提供能力；  
  - 运行时最小集不依赖本仓内容；需要业务/背景文档时，通过配置中的 `doc_root` 指向业务文档目录（可以是本仓或其他 workspace）。

当你修改 langos 的协议/路由/规范时，推荐流程是：

1. 在本仓 `specs/langos/` 中写或更新需求规格；  
2. 必要时在 `adr/` 中记录关键决策；  
3. 再在 `../langos/runtime/` 中修改协议文件与相关文档。

---

## 5. 新增或调整文档时的简要原则

新增或调整文档时，建议遵循：

1. 先确定文档归属的目录（blueprints / repos / specs / projects / adr / domain / archive / meta）；  
2. 参照 `meta/conventions.md` 选定文件名与结构；  
3. 如涉及目录结构变更或分类规则调整，参考 `meta/README.md` 中的“结构变更流程”，必要时配套规格或 ADR；  
4. 更新相关 README 或索引（例如 `specs/README.md`、`repos/INDEX.md`），避免产生孤立文档或断链。

不确定放在哪时，可以先在对话中说明意图，由 AI 协助判断目录，并复述确认后再落盘。  
