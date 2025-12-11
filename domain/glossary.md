# 术语与目录约束（doc_root）

- **doc_root**：业务/背景文档目录（本仓），启动时仅校验存在，不预读内容。
- **repos/**：项目索引与简介目录；每个仓使用单文件 `repos/<name>.md`，包含概览、目录地图、主要导航。不得混入蓝图/治理/规格。
- **specs/**：需求规格目录；跨仓或全局规格直接放在此处，按仓或主题可建子目录（如 `specs/langos/`）。命名建议 `YYYYMM-short-name-spec.md`。
- **projects/**：跨仓或阶段性项目文档目录（overview/plan/milestones/notes）。
- **adr/**：架构决策记录目录，命名 `YYYYMMDD-short-name.md`。
- **domain/**：领域知识、术语表、数据模型等（本文件所在）。
- **archive/**：归档目录，存放过时或完成的文档，需标注来源与日期。
- **运行时最小集**：在代码仓 `langos/runtime/*`，不依赖 doc_root 内容；doc_root 仅在需要业务/背景信息时按需读取。
