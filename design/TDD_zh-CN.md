技术设计文档（TDD）
1. 文档目的

本文档用于说明个人记账系统在实现阶段的技术设计决策，包括架构选择、核心模块职责、关键数据结构与业务逻辑，作为开发与评审的依据。

目标：

保证账务数据一致性与可追溯性

支持国际化、多币种与多人协作

为未来 AI 分析与系统扩展预留清晰接口

2. 系统总体架构
2.1 架构风格

分层架构（Layered Architecture）+ 领域拆分

前后端分离

REST API 为主

2.2 核心模块

Auth Module：认证、授权

Ledger Module：账本与成员管理

Transaction Module：账目录入与查询

Budget Module：预算规则与校验

Report Module：统计与可视化数据

AI Insight Module：支出分析与建议（只读）

3. 技术选型说明
层级	技术	选择理由
Frontend	React + TypeScript	类型安全、生态成熟
Backend	NestJS	模块化、适合复杂业务
DB	PostgreSQL	事务与约束能力强
Cache	Redis	性能与并发支持
AI	LLM API	作为分析能力模块
4. 核心数据结构设计
4.1 账目金额模型

保存字段：

amount_original

currency_original

exchange_rate

amount_converted

设计原因：

防止历史数据因汇率变化被重算

满足金融系统对可审计性的要求

4.2 账本与权限模型

User 与 Ledger 通过 LedgerMember 关联

角色分级：owner / editor / viewer

设计原因：

支持多人共享

权限扩展不影响核心表结构

5. 核心业务逻辑
5.1 记账流程

用户提交账目信息

校验账本权限

查询汇率服务（缓存优先）

计算换算金额

持久化 Transaction

触发预算校验

5.2 预算校验逻辑

以账本 + 周期 + 分类为维度聚合

若超限：

标记状态

返回告警信息（不阻断写入）

5.3 定期支出生成

使用 RecurringTransaction 作为规则模板

定时任务生成真实 Transaction

失败任务可重试

6. 并发与一致性

使用数据库事务保证写入原子性

关键表使用乐观锁（version）

成员权限变更与账目操作解耦

7. AI 模块设计原则

AI 不直接访问原始账目表

只接收聚合后的统计数据

AI 输出仅作为建议，不写入核心账务数据

8. 安全设计

密码使用强哈希（bcrypt）

JWT + Refresh Token

重要操作进行权限校验

AI 与外部 API 隔离

9. 异常与边界处理

汇率服务不可用：使用最近缓存

重复提交：幂等性校验

并发冲突：返回 409

10. 扩展性设计

新币种：无需 Schema 变更

新报表：通过 Report Module 扩展

新 AI 能力：替换 AI Insight 实现

11. 非目标（Out of Scope）

银行账户直连

税务/法律计算

自动记账