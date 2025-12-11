# langos 运行时与文档解耦需求规格

Status: implemented  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: runtime/startup.md、runtime/guidelines.md、runtime/runtime.md、runtime/protocols/index.yaml、meta/README.md、meta/conventions.md

## 背景与目标
- 现状：langos 运行时文档中存在对 docs 仓文档的直接提及/引用，容易让人误以为运行时依赖 docs 才能运转。
- 目标：澄清“docs 是蓝图产出源，运行时产出后应独立可用”；运行时文档与协议不再指向或依赖 docs 内容即可工作。
- 期待效果：治理流程仍可参考 docs，但运行时的启动/指引文本在无 docs 内容可读的情况下也清晰可用；docs 记录蓝图与解耦原则。

## 触发意图
- 治理时发现运行时说明与 docs 耦合，需梳理并调整。
- 需要向维护者强调：docs 仅在设计期使用，运行时应保持独立。

## 范围
- 更新 langos 仓内文档/协议的叙述（如 `runtime/startup.md`、`runtime/guidelines.md`、`runtime/runtime.md`、必要时 `runtime/protocols/index.yaml` 中的描述）以移除对 docs 内容的运行时依赖表述，并强调独立性。
- 更新 docs 仓（如概览、相关规格或新增片段）记录“蓝图→独立内核”的设计原则与维护策略。
- 若有必要，梳理目录映射/启动提示，使无 docs 内容时仍能正常引导。

## 非范围
- 不在运行时代码中加入新的硬性约束或机制（目标是设计/文档层的指导与澄清）。
- 不变更业务协议流程本身，只优化叙述与依赖指引。

## 输入
- 现有运行时文档与协议：`langos/runtime/*`、`langos/runtime/protocols/*.yaml`。
- docs 仓现有蓝图/概览文档：`langos-docs/blueprints/{vision.md,meta-intro.md}`、`repos/langos.md` 及 `langos-docs/specs/langos/` 下的内核规格等。
- 现行启动配置：`.langos-local.yaml` / `.langos.yaml` 中的 `doc_root`（仅用于定位业务/背景文档，运行时需在缺失时也能给出合理提示）。

## 预期输出
- 调整后的运行时文档/协议说明，去除对 docs 内容的运行时依赖表述，并明确独立性。
- 更新后的 docs 片段，记录“docs 产出蓝图，langos 产出后独立运行”的设计思路与维护指引。
- 必要的目录/启动提示修订，让无 docs 内容时仍能顺畅启动。

## 步骤要点
- 盘点运行时文档中所有指向 docs 内容的描述，区分“设计期参考”与“运行期必需”，移除/改写后者。
- 在运行时启动/指引处补充：即便缺少 docs 内容，运行时也能以最小集运行；需要业务文档时再按需指定。
- 在 docs 仓写明：docs 产出蓝图，运行时与 docs 分离；治理可参考 docs，但运行时不可依赖 docs。
- 检查目录映射与配置说明，确保读者理解 doc_root 只是业务/背景文档位置，不是运行时依赖。

## 确认点
- 新旧文档/协议中涉及 docs 的表述是否全部梳理并分类处理。
- “运行时独立可用”与“docs 为蓝图”的核心信息是否在两侧文档都被明确写出。
- 启动提示/模式选择在无 docs 内容时是否仍然清晰。

## 安全与约束
- 仅修改文档/协议描述，不添加运行时硬约束；保持全程中文。
- 修改前后保持协议索引与引用路径正确；不暴露 docs 内容。
- 依旧遵守治理流程：需求确认→方案→落盘→评审。

## 验收标准
- 运行时文档/协议中不存在“必须读取 docs 内容才能工作”的表述，启动提示可在无 docs 内容时自洽。
- docs 仓中新增/更新的说明清晰表达“docs 出蓝图，langos 独立运行”的原则，并给出维护建议。
- 用户/AI 在仅有 langos 仓的情况下也能按文档启动并理解最小运行方式；需要业务文档时按需指定 doc_root。
