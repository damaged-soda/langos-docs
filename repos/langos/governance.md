# LangOS 内核开发与治理规范（背景文档）

本文件描述在“内核模式”下如何安全地修改 LangOS，自举时可参考；运行时最小必读不依赖本文件。

## 1. 核心原则
- **热插拔风险**：你正在修改你赖以运行的规则。如果把 `dispatch-request` 改坏了，你将无法响应后续指令。
- **先模拟后实装**：修改任何 YAML 协议前，必须先调用 `simulate_protocol`。
- **自举循环**：使用 LangOS 的协议来开发 LangOS。

## 2. 标准自举流程 (SOP)
当我们想要增加一个新能力（例如“自动写代码”）时，请遵循以下路径：

1. **需求定义** (利用 `add_requirement`)  
   - 在 `specs/langos/` 下创建需求文档（如 `protocol-implementation-spec.md`）。  
   - 明确 triggers, inputs, outputs, safety。

2. **协议设计** (利用 `add_protocol`)  
   - 生成 YAML 草稿。  
   - **注意**：不要直接覆盖旧文件，先保存为 `runtime/protocols/drafts/` 或仅在内存中持有。

3. **沙盒验证** (利用 `simulate_protocol`)  
   - 针对草稿进行对话模拟。  
   - 修正逻辑漏洞。

4. **实装与索引** (手动或确认后写入)  
   - 将通过测试的 YAML 移入代码仓的 `runtime/protocols/`。  
   - 更新 `runtime/protocols/index.yaml`。

5. **文档同步** (利用 `refresh_repo_docs`)  
   - 扫描代码仓 `runtime/` 目录变化，更新 `runtime.md` 或 `guidelines.md`。

## 3. 目录映射（自举时）
- 业务需求 -> 文档库 `specs/langos/`  
- 业务代码 -> 代码仓 `runtime/protocols/`  
- 业务文档 -> 文档库（本目录） & 代码仓中的运行时文档
