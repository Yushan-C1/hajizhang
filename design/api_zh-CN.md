API 文档（OpenAPI 风格）

版本：v1
认证方式：JWT Bearer Token
数据格式：JSON
Base URL：/api/v1

一、通用约定

1. 认证

Header: Authorization: Bearer <access_token>

未认证返回 401

权限不足返回 403

2. 通用错误码

Code

含义

400

参数错误

401

未认证

403

无权限

404

资源不存在

409

冲突（并发 / 重复）

500

服务器错误

二、Auth API

POST /auth/register

说明：邮箱注册

Request

{
  "email": "user@example.com",
  "password": "string",
  "locale": "zh-CN"
}

Response 201

{ "userId": "uuid" }

POST /auth/login

说明：邮箱登录

Request

{
  "email": "user@example.com",
  "password": "string"
}

Response 200

{
  "accessToken": "jwt",
  "refreshToken": "jwt"
}

POST /auth/refresh

说明：刷新 Token

三、User API

GET /users/me

权限：已登录

Response 200

{
  "id": "uuid",
  "email": "user@example.com",
  "locale": "zh-CN",
  "baseCurrency": "JPY"
}

四、Ledger API

POST /ledgers

说明：创建账本

Request

{ "name": "Personal Ledger" }

Response 201

{ "ledgerId": "uuid" }

GET /ledgers

说明：获取用户账本列表

POST /ledgers/{ledgerId}/invite

说明：邀请成员

Request

{
  "email": "member@example.com",
  "role": "editor"
}

五、Transaction API

POST /transactions

说明：新增账目

Request

{
  "ledgerId": "uuid",
  "amount": 100,
  "currency": "USD",
  "categoryId": "uuid",
  "date": "2026-02-01",
  "note": "Lunch"
}

Response 201

{ "transactionId": "uuid" }

GET /transactions

说明：查询账目

Query Params

ledgerId

startDate

endDate

categoryId

PUT /transactions/{id}

DELETE /transactions/{id}

六、Category API

POST /categories

{ "name": "Food" }

GET /categories

七、Budget API

POST /budgets

{
  "ledgerId": "uuid",
  "categoryId": "uuid",
  "limit": 500,
  "currency": "USD",
  "period": "monthly"
}

GET /budgets/summary

Response 200

{
  "used": 320,
  "limit": 500,
  "status": "NORMAL"
}

八、Report API

GET /reports/summary

GET /reports/pie

Query: period=month

九、Recurring Transaction API

POST /recurring-transactions

{
  "rule": "0 0 1 * *",
  "templateTransactionId": "uuid"
}

十、AI Insight API

POST /ai/insights

说明：获取 AI 支出分析

Request

{ "ledgerId": "uuid", "period": "monthly" }

Response 200

{
  "summary": "You spend more on food than last month",
  "suggestions": ["Set a lower food budget"]
}

