# 统一文档操作协议（doc\_ops）需求规格

Status: draft  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: runtime/protocols/doc-ops.yaml、runtime/protocols/index.yaml、runtime/protocols/README.md、runtime/guidelines.md、runtime/runtime.md、runtime/conventions.md

## 1. 背景与问题

当前 doc\_root（例如 ../langos-docs/）已经形成了较稳定的文档治理结构（SOT + specs 生命周期 + projects 治理项目）。但在“日常文档操作”层面仍存在一个效率与一致性问题：

- 用户的自然语言诉求粒度不固定：
  - 可能是“新增一篇文档”（新 spec / 新 ADR / 新项目文档 / 新 repo 概览等）；
  - 也可能是“在某个文档里加减一两行”（小补丁）；
  - 或者是“整理修订文档”（去重、对齐 SOT、补状态链路、给归档候选清单）。
- 如果为每类诉求单独增加协议，会导致协议数量膨胀、匹配困难，并增加维护成本。
- 如果没有协议统一入口，AI 容易在未盘点/未确认的情况下直接起草或“假设”修改范围，带来治理风险（误改 SOT、误归档、断链、引入新的冲突表述）。

因此需要一个“单协议入口”，既能覆盖上述诉求差异，又能保持强确认与可审计。

## 2. 目标

- 提供一条治理类协议：用户只需表达意图，即可由 AI 在协议内完成：
  - 动作类型判定：create / patch / groom；
  - 范围盘点（inventory）；
  - 变更计划（plan）；
  - 草稿输出（draft：新文档草稿或可复制补丁）；
  - 强确认（confirm）后才允许落盘或执行迁移/归档/重命名。
- 默认 dry-run：在未确认前只输出“计划 + 草稿补丁”，不做任何写入或移动。
- 明确 SOT 优先级规则：运行时 SOT > doc\_root SOT > draft/active specs > 历史（implemented/superseded/archived、ADR、archive）。
- 在不新增多条协议的前提下，为后续细化留出扩展点（在同一协议内增加判定规则、模板与检查单）。

## 3. 非目标

- 不引入一组“按文档类型拆分”的多协议体系（例如 add\_adr/add\_project/add\_domain 等）。
- 不在 v1 中实现自动化工具或 CLI（如批量重命名、自动移动归档），仅定义协议流程与输出。
- 不改变现有 specs 生命周期治理模型，只把它作为 groom 场景下的判定与输出约束。

## 4. 适用范围

- 适用对象：当前会话已选定的 doc\_root 内的文档集合（README、repos、specs、projects、adr、domain、blueprints、meta、archive 等）。
- 典型场景：
  - 新建文档：新增一篇 spec/ADR/项目文档/领域说明/仓库概览等；
  - 小补丁：对既有文档增删改一两段（插入小节、修措辞、删冗余、修链接等）；
  - 整理修订：对齐 SOT、去重、补齐 specs 状态字段与替代链路、给出归档候选清单。

## 5. 核心设计

### 5.1 协议 ID 与定位

- 协议 id：doc\_ops
- role：governance
- 定位：doc\_root 文档操作的统一入口协议（单协议），按诉求在协议内部做分流。

### 5.2 动作类型判定（create / patch / groom）

在协议第一阶段必须显式判定动作类型，并说明判定依据：

- create：
  - 典型表述：“新增一篇…”，“建一个…文档”，“给…补一个说明文档”；
  - 输出重点：建议路径/命名/大纲 + 草稿 + 导航更新片段。
- patch：
  - 典型表述：“在…里加两行”，“把这段删掉”，“改一下措辞/标题/链接”；
  - 必须要求明确目标文件路径与定位锚点（章节名/关键词/段落上下文），避免误改。
- groom：
  - 典型表述：“整理文档”，“去重”，“对齐 SOT”，“spec 太多了梳理一下”，“给归档候选”；
  - 输出重点：盘点清单、冲突/重复/过时点列表、回收进 SOT 的补丁草稿、spec 状态链路补丁、归档候选清单（但不移动）。

### 5.3 默认 dry-run 与强确认闸门

- 默认 dry-run：只允许输出计划与草稿，不允许写入或移动。
- 强确认动作（任何一个出现都必须单独列清并征求确认）：
  - 写入/覆盖文件；
  - 移动文件（含迁移到 archive/）；
  - 重命名文件（可能导致断链）；
  - 批量修改（影响多文件或 SOT 文件）。

## 6. 协议流程（步骤要求）

协议步骤建议固定为：

1) 澄清与判意图（ASK\_USER）
- 给出动作类型候选（create/patch/groom），并最小化提问以锁定范围。
- 若涉及 patch，必须确认目标文件路径与定位锚点。
- 若涉及强动作，默认不允许，除非用户显式授权。

2) 盘点与预读（ANALYZE\_DOCS）
- 在确认范围内列出文件清单（包含/排除）。
- 必须标注 SOT 文件（运行时与 doc\_root 两侧），并优先用于后续判断。
- 不输出冗长摘要，只输出“盘点清单 + 缺项提示”。

3) 计划（PLAN\_DOCS）
- 逐文件列出要改什么、为什么、风险、是否需要 ADR、是否涉及强动作。
- create 必须包含路径与命名理由、文档大纲、导航更新文件列表。
- groom 必须按 SOT 优先级给出回收策略与归档候选（不执行迁移）。

4) 草稿（DRAFT\_DOCS）
- create：给出完整新文档草稿（含 TODO）。
- patch：给出可复制补丁片段，必须包含上下文锚点与替换范围说明。
- groom：输出一组可复制补丁（SOT 修订、spec 状态字段与替代链路补齐等）+ 归档候选清单。

5) 收尾确认（CONFIRM\_AND\_SUMMARIZE）
- 列清：文件路径、草稿/补丁、需要同步更新的导航片段、强确认动作清单。
- 未确认不得落盘；若用户手动落盘，提供最终可复制内容。

## 7. 与现有协议的关系

- doc\_ops 不要求废弃现有协议（add\_requirement / add\_repo\_index / refresh\_repo\_docs / init\_doc\_root）。
- v1 以“统一入口”为目标：
  - 用户如果只说“帮我加个需求 spec”，doc\_ops 可以处理；
  - 运行时也可以继续保留 add\_requirement 的专用路径，两者并存，后续再按使用情况决定是否收敛。
- doc\_ops 的边界：
  - 若诉求是“对照代码更新文档”，可以在 doc\_ops 的判意图阶段建议改用 refresh\_repo\_docs（但不强制）。
  - 若 doc\_root 未初始化或缺关键骨架，必须先走 init\_doc\_root（或在协议内提示并转交）。

## 8. 落盘产物

- 运行时协议文件：
  - runtime/protocols/doc-ops.yaml（新增）
- 协议索引更新：
  - runtime/protocols/index.yaml 增加 doc\_ops 条目与触发例句
- 说明文档（建议同步更新）：
  - runtime/protocols/README.md：补充 doc\_ops 的定位与“单协议入口”说明
  - runtime/guidelines.md 与 runtime/runtime.md：如需，补充“文档操作建议优先尝试 doc\_ops”的提示（不引入强依赖）

## 9. 验收标准

- 路由：
  - 用户以自然语言提出“新增文档 / 小改两行 / 整理修订”等请求时，能稳定匹配到 doc\_ops，并在第一步给出 create/patch/groom 判定。
- 安全：
  - 在用户未确认前，不会写入/覆盖/移动/归档/重命名任何文件。
  - patch 场景必须要求明确目标文件与定位锚点，否则不得起草“会修改原文”的补丁。
- 输出质量：
  - create：输出路径/命名/大纲/草稿/导航片段。
  - patch：输出可复制补丁且包含锚点与替换范围说明。
  - groom：输出冲突/重复/过时清单、回收进 SOT 的补丁草稿、spec 状态链路补丁、归档候选清单（不执行迁移）。
- 一致性：
  - 所有“当前生效规则”建议都以 SOT 为准；spec（implemented/superseded/archived）只用于历史解释，不被当作现行规则。

## 10. 待决事项

- 触发表达的抢占关系：doc\_ops 的 utterance\_examples 需要避免“抢占” init\_doc\_root 或 refresh\_repo\_docs 的典型表达，必要时在 index.yaml 侧调优。
- 命名规范最终口径：spec 文件名是否统一为 YYYYMM-topic-spec.md（建议与 meta/conventions.md 对齐），后续可在 docs 治理项目中统一处理。
