# 协议角色（role）轻量分层方案

Status: implemented  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: runtime/protocols/index.yaml、runtime/protocols/*.yaml（role 字段）、runtime/guidelines.md

## 背景与问题
- 现有所有协议 YAML 视为同层对象，`dispatch_request` 同时承担“核心运行时链路”与“候选协议”双重身份，语义模糊。
- 运行时候选协议列表可能混入内核链路（例如 `dispatch_request`），与“用户可选择的治理/业务协议”混在一起。
- 曾考虑删除 `dispatch_request.yaml` 或将信息迁回文字描述，易丢失结构化规格，不利于内核行为的显式建模。

## 目标与不做事项
- 目标：为协议增加轻量 `role` 维度，区分核心链路与治理/业务协议，保持现有协议结构与最小改动。
- 不做：不删除 `dispatch_request.yaml`，不重写触发/步骤逻辑，不引入破坏性流程改动。

## 范围与角色定义
- 在协议级别声明 `role`：
  - `core`：核心运行时链路（示例：`dispatch_request`），由运行时自动执行，不作为用户候选协议出现。
  - `governance`：治理/文档/配置类协议（示例：`init_doc_root`、`add_requirement`、`add_repo_index`、`refresh_repo_docs`），可被用户自然语言触发并列为候选。
  - 预留：`feature`/`business`（未来业务协议）、`experimental` 等，可后续扩展。
- 作用对象：当前 `runtime/protocols/*.yaml` 及 `runtime/protocols/index.yaml`。

## 方案
1) 协议文件新增字段  
   - 在以下协议 YAML 中增加 `role`：
     - `dispatch_request.yaml` → `role: core`
     - `add-requirement.yaml`、`init-doc-root.yaml`、`add-repo-index.yaml`、`refresh-repo-docs.yaml` → `role: governance`
   - 保持其余字段/结构不变；如未来新增协议，要求填写 `role`，缺省时按下述兼容策略处理。

2) 索引镜像（冗余一段时间）  
   - 在 `runtime/protocols/index.yaml` 对应条目中镜像 `role` 字段，值与协议文件一致。
   - 兼容策略：如协议与索引中的 `role` 不一致，以协议文件为权威，索引待修正；新协议仍建议两处同步写入，后续可视情况去重。

3) 路由与候选过滤规则  
   - 运行时匹配候选协议时，仅考虑 `role != core`（可选：再加 `exposed: true`）的协议进入候选列表。
   - `role = core` 的协议由运行时按内置链路自动执行，不在候选中展示，也不接受用户直接选择。
   - 若协议缺少 `role`，临时视为 `governance`（待补全），并在开发/治理场合提示补写。

4) 文档更新  
   - `runtime/protocols/README.md`：新增“协议角色（role）”小节，说明分类、用途及候选过滤规则。
   - `runtime/guidelines.md`：在守护/分发链路处补充“候选协议仅包含 `role != core`”的明确规则，强调 core 由运行时自动执行。
   - 如有路由/行为示例，按上述规则更新描述，避免 core 协议出现在用户候选示例中。

5) 习惯与命名  
   - 角色枚举保持小写，使用 `role` 字段名（若未来重命名为 `kind`，需同步协议与索引）。
   - 业务协议落盘时遵守 `runtime/conventions.md`（文件命名、确认闸门等）。

## 影响与风险
- 向后兼容：新增字段不破坏现有协议结构；缺失 `role` 时按治理类处理，但需尽快补全以避免语义不明。
- 维护成本：短期需在协议与索引两处维护 `role`（冗余）；需要约定“协议文件为权威”以避免不一致。
- 行为变化：候选列表不再出现 core 协议，需确认运行时筛选逻辑与现有触发/例句不冲突。

## 验收标准
- 协议文件与 `index.yaml` 均新增 `role` 字段，五个现有协议值正确（core: dispatch_request；governance: 其余四个）。
- 路由/候选展示逻辑明确记载：仅 `role != core` 进入候选；core 由运行时内置执行。
- 文档已更新：`runtime/protocols/README.md` 新增角色说明；`runtime/guidelines.md` 增加候选过滤规则。
- 样例/行为描述中不再将 core 协议作为用户可选项。

## TODO / 后续演进
- 枚举扩展：按需加入 `feature`/`business`/`experimental` 等角色，并约定默认值。
- 去重策略：确认后是否移除 `index.yaml` 中的 `role` 冗余，或以协议文件为唯一权威。
- 工具支持：考虑在路由匹配/校验脚本中校验 `role` 存在性与一致性，避免漏填。
