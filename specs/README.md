# specs 目录说明

职责：记录“当前版本的约束/接口/行为”，为开发提供输入。历史决策与取舍放 `adr/`，完全废弃的内容归档到 `archive/`。

- 通用规格直接放 `specs/`，单仓规格放 `specs/<repo>/`（如 `specs/langos/`）。
- 命名建议：`YYYYMMDD-short-name-spec.md`（日期必填到日，避免省略）。
- 编写时明确范围/非范围、输入输出、确认点与验收标准，遵守 `meta/conventions.md`。
- 每篇 spec 顶部标注状态元信息：`Status`（draft/active/implemented/superseded/archived）、`Since`（YYYY-MM），必要时补充 `Supersedes`、`Superseded-by`、`Related-ADR`，并在正文列出预期更新的 SOT 文档；详情见 `specs/langos/20251211-spec-lifecycle-and-doc-grooming-spec.md`。
