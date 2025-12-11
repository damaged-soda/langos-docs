# langos 概览与导航

自然语言驱动的编程“操作系统”内核，运行时代码在 `../langos/`，本文件提供概览与文档导航。

## 目录地图（分层与职责）
```
langos/                 ← 代码仓：运行时/协议/必读规范
├─ runtime/             ← 运行时与协议
│  ├─ guidelines.md     ← 对话流程与行为约束
│  ├─ runtime.md        ← 运行时流程（路由、确认闸门）
│  ├─ startup.md        ← 加载后给人/AI 的启动提示
│  ├─ conventions.md    ← 命名/写作/落盘规范（必读）
│  └─ protocols/        ← 协议及索引（index.yaml、各 *.yaml）
└─ doc_root（本仓）     ← 蓝图/背景/规格，运行时最小集不依赖内容
   ├─ meta-intro.md / vision.md
   ├─ repos/            ← 项目索引与简介（本文件所在）
   ├─ specs/            ← 需求规格（跨仓或按仓子目录，例如 specs/langos/）
   ├─ projects/         ← 跨仓项目文档
   ├─ adr/              ← 架构决策记录
   ├─ domain/           ← 领域知识与术语表
   └─ archive/          ← 归档
```

## 快速使用路径
1) 运行时最小必读（代码仓）：`runtime/startup.md`、`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/index.yaml`、`runtime/conventions.md`。  
2) 蓝图：`meta-intro.md`、`vision.md`。  
3) 规格：`specs/langos/`（内核相关）、`specs/` 其他跨仓规格。  
4) 术语与目录约束：`domain/glossary.md`。  
5) 项目/ADR/归档：见各目录 README。

## 提示
- 业务文档目录（doc_root）可配置；启动时仅校验路径存在，不预读内容。  
- `repos/` 仅承载索引与简介，不放蓝图或规格；新增仓库时按 `repos/<name>.md` 形式补充。  
- 如需治理或理解设计思路，可按需阅读根目录蓝图文档；执行/落盘时遵循代码仓运行时规范。
