# 202512 文档治理项目（spec 生命周期整理）

## 背景
- `specs/langos/20251211-spec-lifecycle-and-doc-grooming-spec.md` 要求对任意 doc_root 的 specs 引入状态模型、SOT 优先级与整理触发点。
- 现有 specs（如 20251211-*）部分尚未标注状态或与 SOT 的映射，缺乏集中清理计划。
- 目的：以 langos 运行时当前使用的 doc_root（默认 `langos-docs`）为起点，建立可复用的整理流程。

## 目标
- 盘点当前 doc_root 的 specs（含子目录）并补全状态元信息（Status/Since/Supersedes/Superseded-by/Related-ADR）。
- 对已落地的改动，更新对应 SOT 文档并将相关 spec 标为 implemented，必要时补充 ADR。
- 标记 superseded/老旧 spec 的替代关系，形成归档清单，确定迁移到 archive/ 的候选。
- 输出可复用的整理清单与节奏（如何扫描、何时归档），供后续 doc_root 复用。

## 范围
- doc_root：当前配置的 doc_root（默认 `../langos-docs/`），如会话中切换其他 doc_root，可复用同一流程。
- 文档类型：specs/ 下的规格文档；涉及 SOT 的 meta/conventions.md、blueprints/*、domain/glossary.md、repos/*.md 及 langos/runtime 文档。

## 非范围
- 不落地自动化工具或协议实现（如 groom_specs CLI/协议）。
- 不处理业务代码仓的实现改动，仅对其对应的文档/SOT 做同步。

## 里程碑/交付
- M1：完成 specs 盘点清单 + 状态元信息补全计划。
- M2：对 implemented/superseded 的 spec 完成 SOT 同步与状态更新。
- M3：形成归档列表并迁移首批 archive/，同时沉淀一份“整理操作手册”在本目录或 meta/。
