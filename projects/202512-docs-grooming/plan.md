# 工作计划（草案）

## 节奏与任务
- 阶段 1：盘点（W2）
  - 列出当前 doc_root specs/ 下的全部文件，记录文件名、主题、当前状态（如缺失标注则记为 pending）。
  - 标识“预计触达的 SOT 文档”与“当前实现状态”（未做/已落地/不确定）。
- 阶段 2：状态与 SOT 对齐（W2-W3）
  - 针对已落地的 spec：更新 SOT（meta/conventions.md、blueprints/*、domain/glossary.md、runtime/* 等），将 spec 状态改为 implemented，并补充 Supersedes/Superseded-by。
  - 发现冲突时先新增/更新 spec + ADR 说明原因，再更新 SOT。
- 阶段 3：归档与治理节奏（W3）
  - 生成归档候选列表：implemented/superseded 且超出时间阈值的 spec。
  - 确认归档动作（迁移 archive/，补充来源/日期/替代路径），整理一份操作手册。

## 清单（更新）
- 盘点表：见 `notes.md`。
- 需对齐的 SOT：`meta/conventions.md`、`meta/README.md`、`blueprints/meta-intro.md`、`blueprints/vision.md`、`domain/glossary.md`、`repos/*.md`、`runtime/conventions.md`、`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/*.yaml`。
- ADR 触发标准：出现架构性决策/取舍时记录，文件头在相关 spec 的 Related-ADR 中引用。
