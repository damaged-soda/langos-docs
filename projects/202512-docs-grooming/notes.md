# 记录与待办

## specs 盘点（初版）
| 文件 | 主题 | 现状/状态标注 | 建议动作 |
| --- | --- | --- | --- |
| specs/langos/20251211-context-init-after-docroot-spec.md | doc_root 选定后自动初始化上下文 | Status: implemented；SOT=runtime/guidelines.md、runtime/runtime.md。 | 已覆盖启动后静默读取 README/repos 等的行为；如后续新增上下文源再同步。 |
| specs/langos/20251211-init-doc-root-protocol-spec.md | init_doc_root 协议 | Status: implemented；SOT=runtime/protocols/init-doc-root.yaml 等。 | 现有协议与索引已含 role/写回规则；后续改动按协议更新。 |
| archive/specs/langos/20251211-langos-docs-classification-spec.md | langos-docs 目录职责/索引调整 | Status: archived（原路径 specs/langos/20251211-langos-docs-classification-spec.md）；SOT=meta/conventions.md、repos/*。 | 归档原因：规则已沉淀到 SOT；后续变更需新 spec/ADR。 |
| specs/langos/20251211-multi-doc-root-selection-spec.md | 多 doc_root 选择 | Status: implemented；SOT=runtime/startup.md、runtime/guidelines.md 等。 | 运行时支持多目录选择并写回 active；保持与配置字段一致即可。 |
| specs/langos/20251211-protocol-role-classification-spec.md | 协议 role 分层 | Status: implemented；SOT=protocols/index.yaml、protocols/*.yaml、runtime/guidelines.md。 | 全部协议已有 role 字段；新增协议时保持。 |
| specs/langos/20251211-langos-runtime-docs-decoupling-spec.md | 运行时与 docs 解耦 | Status: implemented；SOT=runtime/startup.md、runtime/guidelines.md、runtime/runtime.md、protocols/index.yaml、meta/*。 | 已对齐；若发现耦合描述再补 ADR/修订。 |
| specs/langos/20251211-spec-lifecycle-and-doc-grooming-spec.md | spec 生命周期/整理 | Status: active。 | 待完成：SOT 对齐（运行时文档已部分落地）、必要 ADR、治理节奏固化。 |

## SOT 差异与冲突
- 目前未发现需要修正的差异；后续落地时若发现冲突再记录。

## 归档候选
- 已归档：`archive/specs/langos/20251211-langos-docs-classification-spec.md`（原路径：specs/langos/20251211-langos-docs-classification-spec.md；原因：规则已沉淀至 meta/conventions.md 与 repos/ 索引）。
- 待评估：暂无（其余规格均为 2025-12，保持在主线以便查阅/迭代）。
