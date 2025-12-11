# langos-docs

langos 项目的蓝图/规格/治理文档，运行时代码在 `../langos/`，本仓不在运行时最小集内。

## 一句话 SOT（去哪找什么）

- 愿景/分层蓝图 → `blueprints/`（如 `meta-intro.md`、`vision.md`）
- 文档分类/命名规则 → `meta/conventions.md`
- doc_root 与运行时解耦原则 → `blueprints/meta-intro.md` + `specs/langos/langos-runtime-docs-decoupling-spec.md`
- 仓库概览与导航 → `repos/`（如 `repos/langos.md`）
- 需求/设计规格 → `specs/`（仓库专属可用子目录）
- 项目计划/进展 → `projects/`
- 架构决策时间线 → `adr/`
- 术语/数据模型 → `domain/`
- 历史归档 → `archive/`

## 目录职责

- `meta/`：docs 仓自身治理与规范（单一规范源）。
- `blueprints/`：顶层愿景与分层蓝图。
- `repos/`：目标仓库概览与导航，单文件形式 `repos/<name>.md`。
- `specs/`：需求与设计规格；历史取舍不在此展开。
- `projects/`：跨仓或阶段性项目目标、计划与进度。
- `adr/`：架构决策记录，保留时间线。
- `domain/`：术语表、数据模型等概念层信息。
- `archive/`：完全退出主视野的归档，注明来源/替代。

## 快速开始

- 新人建议先读 `meta/README.md` + `meta/conventions.md`。
- 需要理解愿景/分层：`blueprints/meta-intro.md`、`blueprints/vision.md`。
- 查内核规格/导航：`repos/langos.md` + `specs/langos/`。
