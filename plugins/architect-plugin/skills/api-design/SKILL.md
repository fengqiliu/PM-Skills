---
name: api-design
description: API design specifications. 当用户需要设计 API、讨论 RESTful 规范、设计 GraphQL schema 或制定 API 规范时使用此skill。
---

# API Design

API 设计技能，提供 RESTful、GraphQL、gRPC 等主流 API 设计规范和最佳实践。

## API 设计原则

```
┌─────────────────────────────────────────────────────────┐
│                    API 设计原则                         │
├─────────────────────────────────────────────────────────┤
│  1. 一致性 - 统一的命名、格式、错误处理                  │
│  2. 简单性 - 简洁易懂的接口，最小化学习成本              │
│  3. 可发现性 - API 自描述，文档完善                     │
│  4. 演进性 - 版本管理，向后兼容                         │
│  5. 安全性 - 认证鉴权、限流、防爬                        │
└─────────────────────────────────────────────────────────┘
```

## RESTful API 设计

### URL 设计

**资源命名**

```
┌─────────────────────────────────────────────────────────┐
│  ✅ 正确示例                                            │
├─────────────────────────────────────────────────────────┤
│  GET    /users              - 获取用户列表              │
│  GET    /users/{id}         - 获取单个用户              │
│  POST   /users              - 创建用户                  │
│  PUT    /users/{id}         - 更新用户                  │
│  DELETE /users/{id}         - 删除用户                  │
│  GET    /users/{id}/orders  - 获取用户的订单            │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  ❌ 错误示例                                            │
├─────────────────────────────────────────────────────────┤
│  GET    /getUsers                                        │
│  GET    /user/list                                       │
│  POST   /createUser                                      │
│  DELETE /deleteUser?id=123                               │
└─────────────────────────────────────────────────────────┘
```

**命名规范**

| 规则 | 说明 | 示例 |
|------|------|------|
| 小写 | URL 使用小写 | `/users` not `/Users` |
| 复数名词 | 资源使用复数 | `/users` not `/user` |
|  kebab-case | 多词用连字符 | `/order-items` not `/orderItems` |
| 层级嵌套 | ≤3 层 | `/users/{id}/orders/{id}/items` |

### HTTP 方法映射

| 方法 | 用途 | 幂等性 | 安全性 |
|------|------|--------|--------|
| GET | 读取资源 | ✅ | ✅ |
| POST | 创建资源 | ❌ | ❌ |
| PUT | 完整更新 | ✅ | ❌ |
| PATCH | 部分更新 | ❌ | ❌ |
| DELETE | 删除资源 | ✅ | ❌ |

### 状态码规范

| 类别 | 状态码 | 用途 |
|------|--------|------|
| 成功 | 200 OK | 成功返回 |
| | 201 Created | 资源创建成功 |
| | 204 No Content | 删除成功，无返回 |
| 客户端错误 | 400 Bad Request | 请求格式错误 |
| | 401 Unauthorized | 未认证 |
| | 403 Forbidden | 无权限 |
| | 404 Not Found | 资源不存在 |
| | 409 Conflict | 资源冲突 |
| | 422 Unprocessable | 验证失败 |
| | 429 Too Many Requests | 请求过多 |
| 服务端错误 | 500 Internal Server Error | 服务端错误 |
| | 503 Service Unavailable | 服务不可用 |

### 请求/响应格式

**请求格式**

```json
{
  "username": "john",
  "email": "john@example.com",
  "password": "secure123"
}
```

**成功响应**

```json
{
  "success": true,
  "data": {
    "id": "12345",
    "username": "john",
    "email": "john@example.com",
    "createdAt": "2024-01-01T00:00:00Z"
  },
  "meta": {
    "requestId": "req-abc123"
  }
}
```

**错误响应**

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  },
  "meta": {
    "requestId": "req-abc123"
  }
}
```

### 分页设计

```json
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "total": 100,
    "totalPages": 5,
    "hasNext": true,
    "hasPrev": false
  }
}
```

**分页参数**

```
GET /users?page=1&pageSize=20
GET /users?offset=0&limit=20
GET /users?cursor=eyJpZCI6MTJ9
```

### 过滤、排序、字段选择

```
过滤: GET /users?status=active&role=admin
排序: GET /users?sort=createdAt:desc,name:asc
字段: GET /users?fields=id,name,email
组合: GET /users?status=active&sort=createdAt:desc&fields=id,name
```

## GraphQL API 设计

### Schema 设计

```graphql
type User {
  id: ID!
  username: String!
  email: String!
  status: UserStatus!
  orders(first: Int, after: String): OrderConnection!
  createdAt: DateTime!
  updatedAt: DateTime!
}

enum UserStatus {
  ACTIVE
  INACTIVE
  SUSPENDED
}

type OrderConnection {
  edges: [OrderEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type OrderEdge {
  node: Order!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}
```

### Query 设计

```graphql
type Query {
  # 获取用户列表（支持过滤、排序、分页）
  users(
    filter: UserFilter
    sort: [UserSort!]
    first: Int
    after: String
  ): UserConnection!

  # 获取单个用户
  user(id: ID!): User
}

input UserFilter {
  status: UserStatus
  role: Role
  search: String
}

input UserSort {
  field: UserSortField!
  order: SortOrder!
}
```

### Mutation 设计

```graphql
type Mutation {
  # 创建
  createUser(input: CreateUserInput!): CreateUserPayload!

  # 更新
  updateUser(id: ID!, input: UpdateUserInput!): UpdateUserPayload!

  # 删除
  deleteUser(id: ID!): DeleteUserPayload!
}

input CreateUserInput {
  username: String!
  email: String!
  password: String!
}

type CreateUserPayload {
  user: User
  errors: [UserError!]
}
```

### 错误处理

```graphql
type UserError {
  field: [String!]
  code: ErrorCode!
  message: String!
}

enum ErrorCode {
  VALIDATION_ERROR
  NOT_FOUND
  DUPLICATE_EMAIL
  UNAUTHORIZED
}
```

## gRPC API 设计

### Proto 定义

```protobuf
syntax = "proto3";

package user.v1;

service UserService {
  // 获取用户
  rpc GetUser(GetUserRequest) returns (GetUserResponse);

  // 创建用户
  rpc CreateUser(CreateUserRequest) returns (CreateUserResponse);

  // 批量获取用户
  rpc BatchGetUsers(BatchGetUsersRequest) returns (BatchGetUsersResponse);

  // 流式获取用户
  rpc StreamUsers(StreamUsersRequest) returns (stream StreamUsersResponse);
}

message GetUserRequest {
  string id = 1;
}

message GetUserResponse {
  User user = 1;
}

message User {
  string id = 1;
  string username = 2;
  string email = 3;
  string status = 4;
}
```

### 错误处理

```protobuf
message Status {
  int32 code = 1;
  string message = 2;
  repeated ErrorDetail errors = 3;
}

message ErrorDetail {
  string field = 1;
  string message = 2;
}
```

## API 版本管理

### 策略选择

| 策略 | 优点 | 缺点 | 适用场景 |
|------|------|------|---------|
| URL 路径 | 明确、可发现 | 维护多版本 | REST API |
| Header | URL 干净 | 不直观 | 特殊场景 |
| Query 参数 | 灵活 | 容易被忽略 | 过渡期 |

### 版本演进

```
v1 (deprecated) → v2 (current) → v3 (future)
```

### 废弃策略

```json
{
  "success": true,
  "data": {...},
  "deprecation": {
    "active": true,
    "sunsetDate": "2025-12-31",
    "alternative": "/v2/users",
    "message": "Please migrate to v2 API"
  }
}
```

## API 文档模板

```markdown
# API 文档

## 概述
[API 用途和基础 URL]

## 认证
[认证方式：API Key / OAuth2 / JWT]

## 用户 API

### 获取用户列表

```
GET /api/v1/users
```

**请求参数**

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| page | int | 否 | 页码，默认1 |
| pageSize | int | 否 | 每页数量，默认20 |

**响应示例**

```json
{
  "success": true,
  "data": [...]
}
```

**错误码**

| 码 | 说明 |
|----|------|
| 400 | 请求参数错误 |
| 401 | 未认证 |
| 500 | 服务端错误 |

---

### 创建用户

```
POST /api/v1/users
```

**请求体**

```json
{
  "username": "string",
  "email": "string"
}
```

**响应示例**

```json
{
  "success": true,
  "data": {
    "id": "123"
  }
}
```

## 错误响应格式

[统一错误格式说明]
```

## API 设计 Checklist

```
设计前
□ 确定 API 风格（REST/GraphQL/gRPC）
□ 定义资源模型
□ 确定端点列表
□ 设计错误码体系

设计中
□ URL 命名是否遵循规范？
□ HTTP 方法是否正确？
□ 状态码是否一致？
□ 请求/响应格式是否统一？
□ 是否有分页支持？
□ 是否有认证鉴权？

文档
□ 是否有完整的 API 文档？
□ 是否有请求/响应示例？
□ 是否有错误码说明？
□ 是否有 SDK 或客户端库？
```
