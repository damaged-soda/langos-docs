# 为每个仓库生成代码风格/规范文档需求规格

Status: draft  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: `langos/runtime/runtime.md`、`langos/runtime/guidelines.md`、`langos/runtime/protocols/index.yaml`

## 背景与目标

在跨仓开发/治理时，AI 与人类经常遇到“本仓代码风格/分层边界/接口约定是什么”的不确定性，导致：
- 改动风格不一致、评审成本上升；
- 反复在“写代码 ↔ 设计规范”之间横跳；
- 规范散落在 README、口头约定、历史代码中，缺少可检索的单一事实源（SOT）。

目标：提供一套可复用的“生成/维护每个仓库的代码规范文档”的流程，使其成为 repo 层面的常驻 SOT，并可在进入编码阶段前被引用与执行。

## 范围与非范围

- 范围：
  - 为目标 repo 生成一份代码规范文档（Coding Conventions），并给出可执行的检查清单。
  - 支持按 repo 类型生成：后端/前端/脚本/混合仓。
  - 支持“增量更新”：对已存在的规范文档做补丁而非重写。
  - 生成结果可被“编码前闸门”引用（见后续演进），作为进入编码阶段的必读/必守 SOT。
- 非范围（本期不做）：
  - 不强制引入或配置 lint/format 工具（可作为建议输出，但不自动改工程）。
  - 不自动重构全仓以对齐规范（规范先落地，小步演进）。
  - 不做跨 repo 的统一强制规范（每仓可以不同）。

## 产出物（SOT 形态）

每个目标 repo 生成/维护以下之一（由用户选定）：
- A. repo 内文档：`<repo>/doc/conventions.md` 或 `<repo>/docs/conventions.md`（推荐：靠近代码，便于 PR 同步）
- B. doc_root 文档库：`<doc_root>/repos/<repo>.md` 增补“代码规范/分层边界”小节 + 链接到详细规范

规范文档最小结构（建议固定模板）：
1) 适用范围（本仓、目录）  
2) MUST（硬规则：可检查）  
3) SHOULD（推荐做法）  
4) MUST NOT（禁止）  
5) Examples（Good/Bad，短代码片段）  
6) PR Checklist（评审清单）

## 新增协议（建议）

新增协议：`generate_repo_conventions`（名称可调整）

### 触发
用户表达类似：
- “为这个仓库生成代码风格/代码规范”
- “给 <repo> 建一份 Controller/API 风格和分层边界规范”
- “把现有规范整理成一份 SOT”

### 交互与步骤（MVP）

1. ASK：确认目标 repo（相对 work/）、代码语言/框架、规范落点（repo 内 or doc_root）。  
2. ANALYZE：扫描并提取“现状事实”：
   - 目录结构与主要模块；
   - 是否存在 formatter/linter/CI（如 Maven/Checkstyle、ESLint/Prettier 等）；
   - Controller/Service/Repository 边界现状（示例 1–2 个）。
3. DRAFT：按模板输出规范草稿（默认 dry-run，不写入）。  
4. CONFIRM：用户确认写入路径与变更范围。  
5. WRITE：写入/更新规范文档，并在需要时更新导航（如 `repos/<repo>.md` 或 repo README 的链接）。  

### 安全与确认闸门

- 默认 dry-run：未确认前只生成草稿与补丁片段。
- 写入前必须明确确认目标路径，避免误写到其他 repo。

## 验收标准

- 能为指定 repo 生成一份结构化规范文档草稿，并包含至少：
  - API 路径/命名与响应封装约定（如适用）
  - Service/Repository 分层边界约定（如适用）
  - 5–10 条 MUST + 对应 PR Checklist
- 能在用户确认后写入到选定位置，并在 repo 概览/导航处可找到该规范入口。

## 演进方向（后续）

- 与“编码前硬阀门”协议联动：进入编码阶段前要求引用目标 repo 的 conventions SOT，并复述确认。
- 自动生成最小 lint/format 建议清单（仅建议，不自动改工程）。
- 支持多语言仓的分区规范（backend/frontend/scripts 分层）。

