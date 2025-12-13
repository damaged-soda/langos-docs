# 需求实现协议（implement_requirement）规格

Status: draft  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: runtime/protocols/implement-requirement.yaml、runtime/protocols/index.yaml、runtime/guidelines.md、runtime/runtime.md、runtime/conventions.md、runtime/protocols/add-requirement.yaml、runtime/protocols/doc-ops.yaml、meta/conventions.md、specs/README.md

## 1. 背景与问题
- 当前仅有 add_requirement 收集规格、doc_ops 做文档操作，缺少“从已确认需求到方案/开发/评审”的标准协议，执行过程易依赖临时对话，确认闸门缺失。
- 技术方案/设计输出缺少统一落盘位置与命名约定，导致需求到实施的衔接不可追溯、难以复用。
- 不同需求落地路径不一致：谁确认范围、何时进入编码、测试与文档更新如何约束，缺乏统一约定，影响可审计性与复用。

## 2. 目标
- 定义协议 `implement_requirement`：接管已确认的需求规格，完成方案制定、任务分解、编码/测试、评审/验收的闭环，沿用 LangOS 的确认闸门。
- 明确技术方案/设计文档的默认承载位置（`specs/<repo>/`），并给出命名建议，保证需求-方案-实现链条可追溯。
- 与运行时路由集成：可被自然语言触发并在 index.yaml 中注册。

## 3. 非目标
- 不取代 add_requirement（需求收集）或 doc_ops（泛文档操作），而是承接已存在的需求规格。
- 不强制引入自动化 CI/部署工具，仅定义人工/AI 协作流程。
- 不改变 repo 自身的编码规范/分支策略，按目标仓库约定执行。

## 4. 适用范围
- 前置：已选定 doc_root，目标需求已有规格（Status 通常为 draft/active）；目标仓库在 repos/INDEX.md 中可识别。
- 适用于业务/内核需求的落地执行；若 doc_root/仓库信息缺失需先补齐（可提示使用 init_doc_root / add_repo_index）。

## 5. 核心设计

### 5.1 协议定位
- id: implement_requirement
- role: feature（面向需求落地的执行协议）
- 触发：用户表述“帮我实现/落地这个需求/规格”“拿这个 spec 去做开发”“按流程推进这个需求”等。

### 5.2 前置输入与预读
- 必要输入：需求规格路径、目标仓库、期望分支/环境、截止时间或优先级（如有）。
- 预读：doc_root 的 repos/INDEX.md、repos/<repo>.md，目标规格全文；必要时读取相关 SOT（runtime/*.md、协议）以对齐确认闸门。
- 如规格为 draft，需先确认是否按该版本执行或先补充/升为 active。

### 5.3 流程与确认点（建议步骤）
1) Intake & 复核（ASK_USER/ANALYZE）  
   - 复述需求目标/范围/不含、依赖、验收口径；标记缺口与假设；确认 go/no-go。  
2) 方案制定（PLAN_SOLUTION）  
   - 给出技术方案：模块边界、接口/数据流、存储/并发/错误处理、风险与替代方案；列出需查阅的 SOT/现有实现；方案须确认后才能编码。  
3) 任务分解与验证计划（PLAN_TASKS）  
   - 列文件/模块改动清单、里程碑、测试策略（单测/集成/回归）、迁移/兼容方案；标明强动作（破坏性/大规模重构）需再次确认。  
4) 编码与变更说明（DRAFT_CODE / APPLY_PLAN）  
   - 按确认的分解逐步生成补丁，写前先复述预期变更；必要时按文件或批次确认；保证可回滚/可审计。  
5) 测试与自检（TEST_AND_VERIFY）  
   - 运行约定测试或提供可执行命令；记录结果与覆盖范围，未跑通需列明原因与风险。  
6) 文档与规格更新（DOC_UPDATE）  
   - 方案文档落盘：默认写入 `specs/<repo>/YYYYMMDD-topic-solution-spec.md`（或 `-design-spec`），含方案要点/决策/风险/验收。  
   - 若实现导致文档/导航更新，生成 doc_ops/refresh_repo_docs 的补丁草稿；对需求规格更新状态（如从 active → implemented）给出建议。  
7) 收尾与交付（CONFIRM_AND_SUMMARIZE）  
   - 总结改动、测试结果、已/待更新文档、已确认/待确认事项、后续行动；经确认后可落盘或交由人手动合并。

### 5.4 技术方案产出与存放
- 默认承载目录：`specs/<repo>/`，与需求规格同目录，便于链路追溯；不新增新目录层级。
- 命名建议：`YYYYMMDD-<topic>-solution-spec.md` 或 `YYYYMMDD-<topic>-design-spec.md`，遵守 `meta/conventions.md` 的日期与小写连字符规则。
- 内容要素：背景与目标、约束/假设、方案设计（架构/模块/数据/流程）、取舍与备选、风险与缓解、测试/验收口径、需更新的 SOT/文档清单。
- 状态管理：沿用 spec 元信息（Status/Since/Supersedes/Superseded-by/Related-ADR），方案落地后可将需求 spec/方案 spec 调整为 implemented。
- 链接回需求：方案文档需在开头引用对应需求规格路径，需求规格可在后续更新中引用方案文档。

### 5.5 输出物
- 确认的方案与任务分解清单（含风险/假设）。
- 可复制的代码补丁与测试命令/结果。
- 方案文档草稿及其他文档更新草稿，需求规格状态调整建议。
- 结项总结（变更说明、未决事项、后续 TODO）。

### 5.6 与现有协议的关系
- 上游：add_requirement 产出的需求规格是输入；规格缺项可提示回到 add_requirement 或先补齐。
- 文档：涉及文档修改时可在协议内生成 doc_ops/refresh_repo_docs 补丁草稿，但仍需按这些协议的强确认执行。
- 基线：遵守 runtime/guidelines.md、runtime/runtime.md 的确认闸门；候选过滤依赖 index.yaml。

### 5.7 路由与触发例句
- “帮我把这个需求实现掉”“按流程落地 specs/langos/... 这个需求”
- “用标准流程推进这个功能”“帮我做方案→开发→验证”
- “接过 add_requirement 的输出继续做开发”

### 5.8 安全与风险约束
- 未确认不得写入/移动/重命名文件；强动作需单独列清并二次确认。
- 缺信息先问，不编造；假设须显式标注并在方案阶段确认。
- 测试失败或未覆盖的风险必须暴露，不能默认为通过。
- 若发现与现有 SOT 冲突，需提示并给出解决路径（更新 SOT 或新增 spec/ADR）。
- 写入前自检：确认当前工作目录（pwd）与目标路径前缀一致；文档落盘需显式包含 active doc_root 前缀（如 `langos-docs/specs/...`），代码/测试落盘按目标仓库路径（如 `langos/...`），避免裸 `specs/...`。

## 6. 验收标准
- 存在协议文件 `runtime/protocols/implement-requirement.yaml`，步骤覆盖 Intake→方案→任务分解→编码→测试→收尾，包含确认闸门与强动作提示。
- `runtime/protocols/index.yaml` 已登记，role 为 feature（或约定值），包含触发例句。
- 方案文档默认落盘路径与命名建议已明确（specs/<repo>/YYYYMMDD-*-solution-spec.md|design-spec.md），并在 docs 规范中同步写明。
- 运行时文档（runtime/runtime.md 或 protocols/README.md）中简述该协议的定位与默认流程。
- 示例落地：能以已存在的需求规格为输入，输出方案、补丁草稿、测试计划与收尾总结。

## 7. 后续演进
- 视使用情况补充自动化检查单（如常见风险模板）、与 CI/发布流程的挂钩。
- 评估是否与 doc_ops/refresh_repo_docs 的文档更新步骤合并或共享子流程。
