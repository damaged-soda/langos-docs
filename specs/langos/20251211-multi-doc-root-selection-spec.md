# 多文档目录选择需求规格

Status: implemented  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: runtime/startup.md、runtime/guidelines.md、runtime/runtime.md、runtime/protocols/index.yaml

## 背景与目标

- 现状：启动进入业务开发模式时默认使用单个 `doc_root`，无法在会话内切换或添加其他文档目录。
- 目标：支持在进入业务开发时列出/选择多个文档目录，或临时输入新目录并写回配置，选择后再继续需求/方案/开发流程。
- 受众：日常使用 langos 的开发者/文档维护者。

## 范围与非范围

- 范围：启动选择业务开发模式后的 doc_root 选择流程；配置读写；选择结果在本会话内生效并可写回默认配置。
- 非范围：文档目录的内容结构调整（仍由 `langos-docs` 等仓自管）；协议集合与路由逻辑本身（除 doc_root 选择入口外）；权限/ACL 管理。

## 配置与存储

- 配置文件：优先 `.langos-local.yaml`，其次 `.langos.yaml`（仅当本地缺失时读取）。
- 新字段：
  - `doc_roots`: map，`<name>: <path>`，path 相对 langos 仓根或绝对路径，允许 `../` 指向同级 docs 仓。
  - `active_doc_root`: string，可选，记录上次选择的目录名称，作为多目录时的默认项。
- 行为：
  - 运行时临时输入的新目录，写入 `doc_roots`，并更新 `active_doc_root`。
  - 旧的单一 `doc_root` 字段视为废弃；如检测到仅有该字段，可提示“一次性迁移为 doc_roots: {default: <path>}”再覆盖写回。

## 运行时流程（进入业务开发时）

- 读取配置：加载 `doc_roots` 列表与 `active_doc_root`；校验路径存在性（只做存在检查，不预读内容）。
- 分支：
  - **无 doc_roots**：提示“当前未配置文档目录，是否新增？”；用户可输入名称+路径；如路径不存在，询问是否创建并调用现有初始化流程（init-doc-root）。
  - **单一 doc_root**：直接使用该目录并向用户短提示（名称+路径），无需额外确认。
  - **多个 doc_root**：列出名称+路径，标注 `active_doc_root` 作为默认项；询问选择，允许输入新目录（新增即写入并设为 active）。
- 校验与更新：
  - 选择或新增后，若目录不存在且用户同意，创建目录并按 init-doc-root 约定补全骨架；若拒绝创建则要求重新选择。
  - 将确认的名称/路径写回 `.langos-local.yaml` 的 `doc_roots` 与 `active_doc_root`。
  - 会话内的 doc_root 设置应被后续依赖 doc_root 的协议（如 add_requirement/refresh_repo_docs）复用。

## 引导文案示例

- 多目录提示（示例）：
  - “检测到多个文档目录：1) langos-docs -> ../langos-docs/（默认），2) demo-docs -> ../demo-docs/。请输入序号选择，或输入 `name: path` 添加新目录。”
- 空目录提示：
  - “当前未配置文档目录，是否添加？示例输入 `langos-docs: ../langos-docs/`。如目录不存在，可为你创建并初始化骨架。”
- 失效路径提示：
  - “目录 demo-docs -> ../demo-docs/ 不存在或不可写，是否创建？若不创建，请重新选择或输入其他目录。”

## 兼容性与破坏性说明

- 旧的单一 `doc_root` 配置废弃，依赖单路径的调用需改为读取选择结果或 `active_doc_root`。
- 多目录选择引导在进入业务开发模式时新增一段交互，默认不影响咨询模式。

## 边界与风险

- 路径可指向 langos 仓外；需提示用户确认写入/创建的目标路径，避免误写。
- 并发修改 `.langos-local.yaml` 时需以最后确认的选择为准，避免覆盖其他字段。
- 名称重复时需提示覆盖风险；空名称或空路径需阻止写入。

## 验收标准

- 0/1/N 目录场景均有明确提示：无目录时能新增；单目录默认使用；多目录可列表选择并支持新增。
- 选择或新增的目录被写回 `.langos-local.yaml` 的 `doc_roots`，并更新 `active_doc_root`；后续协议读取同一会话的选择。
- 不存在的目录会提示创建并可按 init-doc-root 补齐骨架；拒绝创建时能重新选择。
- 旧 `doc_root` 字段存在时会提示迁移或自动生成默认条目，最终转向 `doc_roots` 方案。
