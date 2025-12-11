# langos 概览与目录地图

自然语言驱动的编程“操作系统”内核，提供对话规范与协议；业务文档不在代码仓。

## 目录地图（分层与职责）
```
langos/                 ← 代码仓：运行时/协议/必读规范
├─ runtime/             ← 运行时与协议
│  ├─ guidelines.md     ← 对话流程与行为约束
│  ├─ runtime.md        ← 运行时流程（路由、确认闸门）
│  ├─ startup.md        ← 加载后给人/AI 的启动提示
│  ├─ conventions.md    ← 命名/写作/落盘规范（必读）
│  └─ protocols/        ← 协议及索引（index.yaml、各 *.yaml）
└─ （业务/背景文档在 docs 仓） ← 高层文档见本仓，业务资产放在独立文档库。
```

## 最小使用路径（人/AI 都可参考）
1) 获取代码仓：`git clone <LANGOS_REPO_URL> langos`（在 `work/` 目录）。  
2) AI 初始化必读：`runtime/guidelines.md`、`runtime/runtime.md`、`runtime/protocols/index.yaml`。  
3) 人/AI 对话入口：按 `runtime/startup.md` 先选模式（学习/治理/业务开发）。  
4) 业务开发时：指定业务文档目录/仓库（如 `langos-docs/` 或其它 docs 仓）；按协议执行需求→方案→编码→评审，关键节点必须确认。  
5) 改规范/协议：阅读 `runtime/conventions.md` 与目标协议，修改后同步 `runtime/protocols/index.yaml`，必要时手动对话模拟关键路径。  

## 其他提示
- 业务知识/需求/仓库说明请放在独立的业务文档目录/仓库（本仓或其他），保持与代码仓分离。  
- 自然语言协议检索入口：`runtime/protocols/index.yaml`；新增协议需先登记。  
- 更高层的愿景/治理/自举说明见本 docs 仓下的其它文件。***
