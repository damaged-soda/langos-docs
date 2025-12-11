# langos-docs 分类与 repo/索引职责调整需求规格

## 背景与目标
- 现状：`repos/` 目录曾用于存放愿景/治理/规格等蓝图文档，且 langos 项目的简介使用二级目录（`repos/langos/overview.md`）而非扁平文件；顶层还混入治理说明。
- 目标：明确 doc_root 的分层与放置规则，保持 `repos/` 仅承载索引/概览且使用单文件（如 `repos/langos.md`），蓝图类文档集中在 `blueprints/`（去除单独的治理文档），规格集中在 `specs/`（按仓或主题分子目录），并提供清晰导航。

## 范围与非范围
- 范围：langos-docs 目录结构、导航 README、文档引用路径的调整与说明沉淀。
- 非范围：代码仓内容、运行时协议与流程，不涉及。

## 期望输出
- 目录规划与放置规则：`blueprints/`（蓝图）、`repos/`（索引/概览，单文件命名 `repos/<name>.md`）、`specs/`（规格，含 `specs/<repo>/`）、`projects/`、`adr/`、`domain/`、`archive/`。
- 更新后的导航文档：根 README、`repos/langos.md`、`specs/README` 等。
- 修正的引用路径：运行时与文档中指向蓝图/规格的链接指向新路径。

## 当前调整（已执行）
- 将 `meta-intro.md`、`vision.md` 收纳到 `blueprints/`；移除单独的治理文档。
- 将 langos 内核规格迁移到 `specs/langos/`，保留 README 并更新索引。
- 补齐分类目录的 README（projects/adr/domain/archive/specs），并改用 `repos/langos.md` 承载索引/概览。
- 更新运行时与文档中的引用到新路径。

## 待补/验收标准
- `repos/` 仅保留索引与概览，不再混入蓝图/规格类文档。
- 文档内引用无断链（至少涵盖根 README、meta-intro、vision、规格、运行时说明）。
- 未来新增文档遵循本分类规则，若需变更结构需更新 README/索引。
