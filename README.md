# langos-docs

langos 项目的高层文档库（蓝图/背景/规格），代码仓在 `../langos/`，运行时最小必读仍在代码仓。

## 阅读顺序
1) `repos/INDEX.md` + `repos/langos.md`：了解项目列表和 langos 概览。  
2) 蓝图：见 `blueprints/`（`meta-intro.md`、`vision.md`）。  
3) 需求规格：跨仓或全局放在 `specs/`（langos 内核相关在 `specs/langos/`）。  
4) 项目/ADR/领域/归档：对应目录的 README 说明使用方式。  
5) 执行/编码时，请回到代码仓按 `runtime/startup.md`、`guidelines.md`、`runtime.md`、`protocols/index.yaml` 进行。

## 目录骨架
- `blueprints/`：蓝图类文档（meta-intro、vision 等）。  
- `repos/`：项目索引与简介，按单文件命名 `repos/<name>.md`（例如 `repos/langos.md`）。  
- `specs/`：跨仓或全局需求规格；按仓或主题可分子目录（如 `specs/langos/`）。  
- `projects/`、`adr/`、`domain/`、`archive/`：预留分类，README 已说明放置规则；术语/目录约束见 `domain/glossary.md`。  

> 提醒：doc_root 仅用于按需读取业务/背景文档；运行时最小集不依赖本仓内容。
