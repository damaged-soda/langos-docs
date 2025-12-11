# langos 概览与导航

自然语言驱动的编程“操作系统”内核，运行时代码在 `../langos/`，本文件提供概览与文档导航。

## 目录地图（分层与职责）

```text
langos/                  ← 代码仓：运行时/协议/必读规范
├─ runtime/              ← 运行时与协议
│  ├─ guidelines.md      ← 对话流程与行为约束
│  ├─ runtime.md         ← 运行时流程（路由、确认闸门）
│  ├─ startup.md         ← 加载后给人/AI 的启动提示
│  ├─ conventions.md     ← 运行时命名/写作/落盘规范（必读）
│  └─ protocols/         ← 协议及索引（index.yaml、各 *.yaml）
└─ doc_root（可配置）    ← 业务/背景文档根目录
   ├─ blueprints/        ← 愿景与分层蓝图（例如本仓默认的 langos-docs/blueprints/）
   ├─ repos/             ← 项目索引与简介
   ├─ specs/             ← 需求与设计规格（如 specs/langos/）
   ├─ projects/          ← 跨仓项目文档
   ├─ adr/               ← 架构决策记录
   ├─ domain/            ← 领域知识与术语表
   ├─ archive/           ← 归档
   └─ meta/              ← 文档仓自身的治理与规范（如 langos-docs/meta/）


**说明**
- 实际配置中的 doc_root 可以指向 ../langos-docs/，也可以指向其他 workspace 的文档目录。
- langos 运行时在启动时仅校验 doc_root 是否存在，不预读取或暴露内容。
- 运行时最小集完全在 langos/runtime/* 内部，不依赖 doc_root 内容。

## 快速使用路径

1. 运行时最小必读（代码仓 `langos/`）
   - `runtime/startup.md`：加载后模式选择与对话入口。
   - `runtime/guidelines.md`：对话行为底线。
   - `runtime/runtime.md`：运行时流程与确认闸门。
   - `runtime/protocols/index.yaml`：协议索引与触发例句。
   - `runtime/conventions.md`：运行时命名、写作、落盘规范。
2. 蓝图与 docs 仓规范（doc_root 侧）
   - 若 doc_root 指向 `../langos-docs/`，可参考：
     - `meta/README.md`：langos-docs 仓的定位与迭代规则。
     - `meta/conventions.md`：docs 仓文档的结构、命名、分类规范。
     - `blueprints/meta-intro.md`、`blueprints/vision.md`：愿景与分层蓝图。
3. 规格与项目文档
   - `specs/langos/`：langos 内核相关规格（例如 docs 分类、运行时与 docs 解耦等）。
   - 其他跨仓规格位于 `specs/`。
   - `projects/`：跨仓或阶段性治理/开发项目文档。
   - `adr/`：关键架构决策记录。
4. 术语与目录约束
   - `domain/glossary.md`：doc_root、repos、specs、projects、adr、domain、archive 等术语说明。
   - 具体目录与命名规范以 `meta/conventions.md` 为准。

## 提示

- 业务文档目录（doc_root）通过配置指定，示例配置可见 `../langos/.langos-local.example.yaml`。
- doc_root 可设置为 `../langos-docs/` 指向本仓，也可以指向其他 workspace（如 `../workspace-docs/`）。
- doc_root 的目录结构/命名/分类规则以 doc_root 所在文档库的规范为准（例如 `langos-docs/meta/conventions.md` 为单一规范源），本文件不重复这些规则。
- `repos/` 仅承载项目索引与简介，不放蓝图或规格；新增目标仓库时按 `repos/<name>.md` 形式补充。
- 若需要理解设计思路或迭代规范，可按需参考 doc_root 下的蓝图与 specs；执行或落盘时以代码仓的运行时协议与规范为准。
- 治理 langos 本身时（内核模式），可以把 `langos/` 当作目标 repo，把 doc_root（如 `../langos-docs/`）当作治理时参考的文档库，按“需求 → 方案 → 编码/落盘 → 评审”的流程小步推进。
