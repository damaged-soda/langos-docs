# langos-docs 分类与 repo/索引职责调整需求规格

## 背景与目标

- 现状回顾：  
  - `repos/` 目录曾混入愿景/治理/规格等蓝图文档，且 langos 项目的简介使用二级目录（`repos/langos/overview.md`）而非常规的扁平文件；  
  - 顶层 README 与若干文档中，对 doc_root、目录职责、运行时解耦的说明存在重复与轻微不一致。  
- 本次目标：  
  - 明确 doc_root 的分层与放置规则，保持 `repos/` 仅承载“项目索引/概览”；  
  - 引入单独的 `meta/` 目录，用于承载 **langos-docs 仓自身的治理与规范**；  
  - 将蓝图类文档集中到 `blueprints/`，规格集中到 `specs/`，目录职责由 `meta/conventions.md` 统一定义，并通过少量 README 与 glossary 做导航；  
  - 减少 doc_root/解耦 相关描述在多处重复，统一到少数“源头文档”中。

## 范围与非范围

- 范围：
  - langos-docs 仓的目录结构与 README 导航；  
  - `repos/`、`specs/`、`domain/`、`meta/` 等目录的职责说明与引用路径；  
  - 与 doc_root、运行时解耦有关的说明在 docs 侧的归拢与调整。
- 非范围：
  - `../langos/` 仓内的代码实现与协议行为本身；  
  - 业务仓或其他 workspace 的具体结构，不在本规格中约束。

## 期望输出

- 目录规划与放置规则：
  - `meta/`：langos-docs 仓自身的治理与规范（例如迭代规则与 docs-conventions），作为 docs 侧的“单一规范源”；  
  - `blueprints/`：蓝图文档（愿景、分层定位、meta-intro 等）；  
  - `repos/`：项目索引与概览，仅使用单文件形式 `repos/<name>.md` 承载每个目标仓的 overview；  
  - `specs/`：需求与设计规格，按仓或主题可分子目录（如 `specs/langos/`）；  
  - `projects/`：跨仓或阶段性项目文档；  
  - `adr/`：架构决策记录；  
  - `domain/`：领域知识与术语解释（如 `glossary.md`）；  
  - `archive/`：归档目录，存放已完成或过时文档。
- 更新后的导航文档：
  - 根 README：说明 langos-docs 仓的定位与高层导航，并指向 `meta/`、`blueprints/`、`repos/`、`specs/` 等；  
  - `meta/README.md`：说明如何迭代 langos-docs 仓；  
  - `meta/conventions.md`：定义 docs 仓内的结构/命名/分类规范；  
  - `specs/README.md`：说明规格目录的用途与命名建议；  
  - `repos/langos.md`：承载 langos 内核项目的 overview 与导航（包括指向 `../langos/` 与本仓相关文档）。
- 修正与统一的说明：
  - 与 doc_root、运行时解耦相关的核心说明，在 docs 侧以 `blueprints/meta-intro.md` 与本规格为主；  
  - 术语解释集中在 `domain/glossary.md`，并引用 `meta/conventions.md` 作为规范来源。

## 当前调整（已执行）

- 目录与文件：
  - 将 `meta-intro.md`、`vision.md` 收纳到 `blueprints/`，用于承载顶层蓝图；  
  - 新增 `meta/README.md`，说明 langos-docs 仓的定位与迭代规则；  
  - 新增 `meta/conventions.md`，统一约束本仓文档的结构、命名与分类；  
  - 将 langos 内核相关规格统一迁移到 `specs/langos/`，并在 `specs/README.md` 中说明用法；  
  - 为 `projects/`、`adr/`、`domain/`、`archive/`、`specs/` 等目录补充 README，用于说明目录职责与基本用法；  
  - 用扁平文件 `repos/langos.md` 承载 langos 内核的 overview 与导航，替代旧的二级目录结构。
- 文档与说明：
  - 更新根 README，强调 langos-docs 是蓝图/规格仓，运行时最小集在 `../langos/`，不依赖本仓内容；  
  - 在 `domain/glossary.md` 中保留术语解释，将具体规范迁移至 `meta/conventions.md`；  
  - 在 `blueprints/meta-intro.md` 中明确：运行时规范在 `langos/runtime/conventions.md`，docs 仓规范在 `meta/conventions.md`。

## 待补与验收标准

- 待补工作：
  - 对照 `langos/runtime/*.md` 与 `langos-docs/blueprints/meta-intro.md`，统一 doc_root 与运行时解耦的表述，避免在多处以不同措辞重复解释；  
  - 根据实际演进情况，对本规格进行小幅更新，使“已执行”的描述保持真实；  
  - 如有新的目录类型或分类需求出现，应先更新 `meta/conventions.md` 与相关 README，再在本规格中记录变更原因。
- 验收标准：
  - `repos/` 仅保留项目索引与概览，不再混入蓝图/规格类文档；  
  - 所有目录的职责说明以 `meta/conventions.md` 与各目录 README 为准，`domain/glossary.md` 只做术语解释；  
  - 文档内与 doc_root、运行时解耦、目录职责相关的说明无明显冲突，至少在根 README、`meta-intro`、本规格以及运行时说明中保持一致心智模型；  
  - 未来新增文档遵循本分类规则，如需结构变更则同步更新 `meta/conventions.md` 和本规格。
