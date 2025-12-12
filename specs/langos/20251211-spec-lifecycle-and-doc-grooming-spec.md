# spec 生命周期与文档整理治理需求规格

Status: active  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -

## 1. 背景与问题

- langos 运行时可配置多个文档目录（doc_root），常见如 langos-docs，但未来可能有其他业务/治理文档库。
- 各 doc_root 的 specs/ 用于“需求与设计规格”，随着协议/结构调整会持续新增并积累历史。
- 问题：旧 spec 设定失效但仍存在，冲突/继承缺少标注；查“当前规则”易被旧 spec 干扰；文档整理缺少明确触发时机与流程约束。

## 2. 目标与非目标

### 2.1 目标

- 为任意 doc_root 下的 specs/ 引入统一生命周期/状态模型，区分草稿、在执行、已落地、被取代等。
- 明确单一真相源（SOT）优先级：当前生效规则集中在各 doc_root 内的少数 SOT 文档（meta/、blueprints/、domain/ 等）以及运行时文档（langos/runtime/*），spec 主要作为变更输入与历史记录。
- 规定必触发“文档整理”的时机：新增/修改 spec、对应改动实现完成、启动文档治理项目。
- 给 AI 行为设规则：查“当前规则”时优先看 SOT，仅在需要历史/动机时参考旧 spec/ADR/archive。

### 2.2 非目标

- 不直接定义工具或协议实现（如 groom_specs 协议/CLI），可后续单独写 spec。
- 不要求一次性清理存量 spec，重点约束后续演进；存量可按项目逐步对齐。
- 不对业务代码仓自身的 specs 目录做强制结构约束，但建议沿用本状态模型和整理流程。

## 3. 范围与适用对象

- 适用目录：当前工作区内由 langos 运行时选定/使用的任意 doc_root 下的 specs/（含子目录，如 specs/langos/、specs/<repo>/）。
- 相关 SOT：
  - 各 doc_root 内：meta/README.md、meta/conventions.md、blueprints/*.md、domain/*.md、repos/*.md。
  - 运行时侧（跨 doc_root 通用）：langos/runtime/*.md、langos/runtime/protocols/*.yaml + index.yaml。
- 适用角色：维护 langos 内核与 docs 仓的开发者；内核模式下的 AI 在解释/修改协议与规范时需遵守。
- 不适用：非 langos 运行时管理的其他文档仓（可参考思路自选）。

## 4. 核心概念与术语

### 4.1 SOT（Single Source of Truth）

- 当前生效规则/接口的唯一权威来源，优先级高于 specs。
- 典型 SOT：
  - doc_root 内：meta/conventions.md、meta/README.md、blueprints/meta-intro.md、blueprints/vision.md、domain/glossary.md、repos/<name>.md 等。
  - 运行时侧：langos/runtime/conventions.md、langos/runtime/guidelines.md、langos/runtime/runtime.md、langos/runtime/protocols/*.yaml + index.yaml。

### 4.2 spec 文档

- 描述一次具体变更/能力的需求与设计输入，每篇聚焦一个主题；本身不是长期 SOT，需在合适时机沉淀到 SOT。

### 4.3 spec 状态字段

- 统一在文档头部标注：
  - Status: draft | active | implemented | superseded | archived
  - Since: YYYY-MM
  - Supersedes: <旧 spec 文件名或 ID，可选>
  - Superseded-by: <新 spec 文件名或 ID，可选>
  - Related-ADR: <ADR 文件名列表，可选>
- 含义：
  - draft：草稿，尚未确认可执行。
  - active：已确认执行/进行中，尚未完全落地。
  - implemented：变更已完全落地，当前行为以 SOT 为准，spec 供追溯。
  - superseded：已被新 spec 取代，不再用于未来设计。
  - archived：迁入 archive/，仅保留历史记录。

## 5. 触发场景与行为要求

### 5.1 新增/修改 spec

- 触发：在任一 doc_root 的 specs/ 下新建或大幅修改 spec。
- 要求：
  - 头部填状态元信息：新建用 draft/active，写 Since，必要时写 Supersedes。
  - 正文明确：背景/目标、范围/非范围、预期将更新的 SOT 列表（指向对应 doc_root 与运行时侧 SOT）。
  - commit/PR 描述简述：新能力或规则修订、是否需要 ADR。

### 5.2 对应改动实现完成（强制整理）

- 触发：spec 描述的改动已在运行时或对应 doc_root 中落地。
- 要求：
  - 更新对应 SOT：按 spec 约定的 SOT 列表逐一落实，确保当前规则可在 SOT 独立阅读。
  - 更新 spec 状态：改为 implemented；如替代旧 spec，填写 Supersedes 并在旧 spec 写 Superseded-by。
  - 视重要性生成 ADR（架构性决策/取舍时），并在 spec 头标 Related-ADR。
  - 为后续治理项目提供待归档候选（见 5.3）。

### 5.3 文档治理项目（集中整理）

- 触发：维护者认为 specs 堆积或需集中清理，在 projects/ 下建治理项目目录（如 202512-docs-grooming/）。
- 目标/动作示例：
  - 扫描各 doc_root 的 specs/，找出 implemented/superseded 且时间早于阈值的 spec。
  - 检查对应 SOT 是否已更新充分。
  - 对已完全沉淀到 SOT 且无需频繁查阅的 spec：
    - 重要决策/权衡 → 提炼/合并为 ADR。
    - 其他 → 迁移 archive/，头部注明原路径、归档原因、归档日期、替代文档路径（指向 SOT）。

## 6. 单一真相源优先级与冲突处理

- 优先级（高→低）：
  1) 运行时行为与接口：langos/runtime/conventions.md、langos/runtime/guidelines.md、langos/runtime/runtime.md、langos/runtime/protocols/*.yaml + index.yaml。
  2) doc_root 内 SOT：meta/conventions.md、meta/README.md、blueprints/meta-intro.md、blueprints/vision.md、domain/glossary.md、repos/*.md 等。
  3) 进行中变更输入：各 doc_root 的 specs/ 下 Status ∈ {draft, active}。
  4) 历史决策与过程：adr/；specs/ 下 Status ∈ {implemented, superseded, archived}；archive/。
- 冲突处理：当 SOT 与 spec/ADR 描述冲突时，以等级 1/2 SOT 为准；spec/ADR 视为待回收/待修订，需通过新增/更新 spec + ADR 记录“为什么 SOT 被改写”，不无痕重写。

## 7. 面向 AI 的行为约束（需求）

- 回答“当前规范/规则”时：先查运行时 SOT（等级 1）和当前 doc_root 的 SOT（等级 2）。
- 引用 spec 内容前检查 Status：仅 draft/active 且明确适用当前迭代的内容可作为“即将生效的设计输入”；implemented/superseded/archived 仅用于历史解释，不得当成当前规则。
- 发现 SOT 与 spec/ADR 冲突时：提示存在不一致，建议通过新增/更新 spec + ADR 梳理；除非用户明确指定，不自行假设正确版本。

## 8. 验收标准（初版）

- 任一 doc_root 的 specs/ 新增的 spec 头部都有 Status/Since 等元信息，正文列出预期更新的 SOT 列表。
- 最近落地的 spec（如 20251211-init-doc-root-protocol-spec.md、20251211-multi-doc-root-selection-spec.md 等）对应 SOT 已更新，且这些 spec 的状态为 implemented，替代关系已填。
- 在 doc_root 的 meta/conventions.md 或 specs/README.md 中补充了：spec 状态字段的使用约定、SOT 优先级规则的简述。
- 至少有一次文档治理项目实例：projects/ 下存在治理项目目录，记录清理旧 spec；部分历史 spec 已迁移 archive/，文件头含来源/归档原因/替代路径。
