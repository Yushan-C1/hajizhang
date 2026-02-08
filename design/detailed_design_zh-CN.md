1️⃣ API 设计（示例）
Auth
POST /auth/register
POST /auth/login
POST /auth/refresh

Ledger
POST   /ledgers
GET    /ledgers/{id}
POST   /ledgers/{id}/invite

Transaction
POST   /transactions
GET    /transactions?date=&category=&currency=
PUT    /transactions/{id}
DELETE /transactions/{id}

Budget
POST /budgets
GET  /budgets/summary

Report
GET /reports/summary
GET /reports/pie?period=month

AI
POST /ai/insights

2️⃣ 数据库 Schema（核心表）
User
User(
  id PK
  email UNIQUE
  password_hash
  locale
  base_currency
  created_at
)

Ledger
Ledger(
  id PK
  name
  owner_id FK(User)
)

LedgerMember
LedgerMember(
  ledger_id FK
  user_id FK
  role (owner/editor/viewer)
)

Transaction
Transaction(
  id PK
  ledger_id FK
  amount_original
  currency_original
  amount_converted
  base_currency
  exchange_rate
  category_id FK
  date
  note
)

Category
Category(
  id PK
  name
  user_id
)

Budget
Budget(
  id PK
  ledger_id
  category_id (nullable)
  limit_amount
  period (monthly)
)

RecurringTransaction
RecurringTransaction(
  id PK
  rule (cron-like)
  template_transaction_id
  active
)

3️⃣ 核心业务逻辑（Critical Logic）
汇率处理

记录原始金额

记录当时汇率

禁止后算（防止历史失真）

汇率缓存 + fallback

多人并发

乐观锁（version）

冲突检测 → 提示用户

预算计算

按 period 聚合

分类优先于整体

实时计算 + 定时校验

AI 数据隔离

只提供聚合数据

不传个人明细

可关闭 AI 功能