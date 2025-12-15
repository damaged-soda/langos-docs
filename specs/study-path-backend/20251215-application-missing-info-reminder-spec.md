# Application 缺失材料提醒（站内）需求规格（MVP1：消息表 + 新 API）

Status: draft  
Since: 2025-12  
Supersedes: -  
Superseded-by: -  
Related-ADR: -  
SOT: `study-path-backend` 的 application 实体与管理端更新接口

## 背景与目标

- 现状：Admin 发现 application 缺失材料/信息时，只能写在 notes（自由文本）里；用户不一定知道需要补什么。
- 目标：提供一个“站内可见的缺失提醒”能力：Admin 创建一条“申请消息”，用户登录网站后能看到该消息并知道下一步要补什么。

## 范围与非范围

- 范围：
  - 新增一张“申请消息表”，用于存放 Admin 写给用户的缺失提醒（不引入类别/多渠道）。
  - 新增管理端 API：创建/关闭（resolved）某条申请消息。
  - 用户侧（站内）可查询/展示该 application 的“未关闭消息”（具体 UI 暂只要求“能看到”，样式后续迭代）。
- 非范围：
  - 不发送微信模板消息。
  - 不做通知历史/多条消息队列（不新增 `user_notification` 表）。
  - 不从 notes 文本中自动解析缺失项。

## 用户流程（最小）

1. Admin 打开某个 application。
2. Admin 新增一条“申请消息”（例如“缺失英语成绩单，请登录补充”）并保存。
3. 用户登录网站后，在申请详情页（或其他约定位置）看到该消息，并按指引去补材料。
4. 用户补齐后，Admin 将该消息标记为“已关闭/已处理”（resolved）。

## 数据模型（新增：独立消息表）

新增表：`application_message`（名称按现有 JPA/表命名习惯调整）

最小字段：
- `id`: UUID / bigint
- `application_id`: UUID
- `user_id`: UUID（消息接收方；用于鉴权与查询）
- `message`: text（缺失提醒文案）
- `created_by`: UUID（admin）
- `created_at`: timestamp（消息诞生时间）
- `updated_at`: timestamp（通用更新时间；close 等状态变更会更新）
- `status`: enum（`OPEN`/`CLOSED`）

约束：
- `message` 不允许为空。
- `application_id` 必须指向存在的 application。
- `user_id` 必须等于该 `application_id` 的所属用户（写入时校验）。
- 同一个 `application_id` 允许存在多条 `status=OPEN` 的消息。

## 接口变更（建议）

管理端：
- `POST /api/admin/applications_messages`
  - body: `{ applicationId: UUID, message: string }`
  - effect: 创建一条申请消息（`status=OPEN`）
- `POST /api/admin/applications_messages/close`
  - body: `{ messageId: UUID }`
  - effect: 将该消息状态更新为 `CLOSED`
  - note: 允许重复 close（幂等）；若已为 `CLOSED` 则返回成功

用户侧读取（建议之一）：
- `GET /api/application_messages/{applicationId}`
  - effect: 返回该 application 的未关闭（`OPEN`）消息列表（供前端展示）

## 安全与权限

- 仅 `admin` 允许写入/更新缺失提醒字段。
- `user` 仅能读取自己的 application 的缺失提醒字段。

## 验收标准

- Admin 可为某个 application 创建一条申请消息，并可关闭该消息。
- 用户在站内可查看自己 application 的“未关闭消息”列表，并能看到消息文案。
- 消息被关闭后，用户侧不再出现在“未关闭消息”列表中。
