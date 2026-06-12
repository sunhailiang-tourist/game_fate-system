# 因果修真世界 - API接口设计

## 一、接口概述

### 1.1 设计目标
API接口设计旨在实现：
- **RESTful设计**：遵循REST原则
- **统一规范**：统一的请求/响应格式
- **高可用**：支持大规模并发
- **安全可靠**：认证授权机制

### 1.2 接口分类

| 分类 | 说明 | 前缀 |
|------|------|------|
| **玩家接口** | 玩家数据管理 | /api/v1/player |
| **世界接口** | 世界状态管理 | /api/v1/world |
| **战斗接口** | 战斗系统 | /api/v1/combat |
| **交易接口** | 经济交易 | /api/v1/trade |
| **任务接口** | 任务系统 | /api/v1/task |
| **AI接口** | AI NPC行为 | /api/v1/ai |

---

## 二、接口规范

### 2.1 请求格式

```json
{
  "timestamp": 1234567890,
  "version": "1.0",
  "data": {}
}
```

### 2.2 响应格式

```json
{
  "code": 200,
  "message": "success",
  "data": {},
  "timestamp": 1234567890
}
```

### 2.3 错误码

| 错误码 | 说明 | 处理方式 |
|--------|------|----------|
| 200 | 成功 | 返回数据 |
| 400 | 请求错误 | 检查参数 |
| 401 | 未认证 | 重新登录 |
| 403 | 未授权 | 检查权限 |
| 404 | 资源不存在 | 检查路径 |
| 500 | 服务器错误 | 重试或联系客服 |

---

## 三、玩家接口

### 3.1 登录接口

| 属性 | 说明 |
|------|------|
| **路径** | POST /api/v1/player/login |
| **描述** | 玩家登录 |
| **认证** | 不需要 |

**请求体：**
```json
{
  "username": "string",
  "password": "string"
}
```

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "token": "string",
    "player": {
      "id": "uuid",
      "name": "string",
      "level": 1,
      "karma": 0
    }
  }
}
```

### 3.2 获取玩家信息

| 属性 | 说明 |
|------|------|
| **路径** | GET /api/v1/player/:id |
| **描述** | 获取玩家信息 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "avatar": "url",
    "level": 1,
    "exp": 0,
    "karma": 0,
    "gold": 0
  }
}
```

### 3.3 更新玩家信息

| 属性 | 说明 |
|------|------|
| **路径** | PUT /api/v1/player/:id |
| **描述** | 更新玩家信息 |
| **认证** | JWT |

**请求体：**
```json
{
  "name": "string",
  "avatar": "url"
}
```

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "avatar": "url"
  }
}
```

---

## 四、世界接口

### 4.1 获取世界状态

| 属性 | 说明 |
|------|------|
| **路径** | GET /api/v1/world/state |
| **描述** | 获取当前世界状态 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tick": 1000,
    "entropy": 0.5,
    "regions": []
  }
}
```

### 4.2 获取区域信息

| 属性 | 说明 |
|------|------|
| **路径** | GET /api/v1/world/region/:id |
| **描述** | 获取区域信息 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "spiritual_density": 1.0,
    "stability": 0.8,
    "entities": []
  }
}
```

---

## 五、战斗接口

### 5.1 发起战斗

| 属性 | 说明 |
|------|------|
| **路径** | POST /api/v1/combat/attack |
| **描述** | 发起战斗 |
| **认证** | JWT |

**请求体：**
```json
{
  "target_id": "uuid",
  "skill_id": "uuid"
}
```

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "battle_id": "uuid",
    "result": "win/lose/draw",
    "damage": 100,
    "rewards": []
  }
}
```

---

## 六、交易接口

### 6.1 创建交易

| 属性 | 说明 |
|------|------|
| **路径** | POST /api/v1/trade/create |
| **描述** | 创建交易 |
| **认证** | JWT |

**请求体：**
```json
{
  "target_player_id": "uuid",
  "offer_items": [],
  "request_items": []
}
```

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "trade_id": "uuid",
    "status": "pending"
  }
}
```

### 6.2 接受交易

| 属性 | 说明 |
|------|------|
| **路径** | POST /api/v1/trade/:id/accept |
| **描述** | 接受交易 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "trade_id": "uuid",
    "status": "completed"
  }
}
```

---

## 七、任务接口

### 7.1 获取任务列表

| 属性 | 说明 |
|------|------|
| **路径** | GET /api/v1/task/list |
| **描述** | 获取任务列表 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tasks": []
  }
}
```

### 7.2 接受任务

| 属性 | 说明 |
|------|------|
| **路径** | POST /api/v1/task/:id/accept |
| **描述** | 接受任务 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task_id": "uuid",
    "status": "accepted"
  }
}
```

---

## 八、AI接口

### 8.1 获取NPC行为

| 属性 | 说明 |
|------|------|
| **路径** | GET /api/v1/ai/npc/:id/action |
| **描述** | 获取NPC行为 |
| **认证** | JWT |

**响应体：**
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "npc_id": "uuid",
    "action": "string",
    "target": "uuid"
  }
}
```

---

## 九、WebSocket接口

### 9.1 实时事件推送

| 属性 | 说明 |
|------|------|
| **路径** | ws://api.example.com/ws/events |
| **描述** | 实时事件推送 |
| **认证** | JWT |

**消息格式：**
```json
{
  "type": "combat_update",
  "data": {}
}
```

---

## 十、开发注意事项

### 10.1 认证授权
- 使用JWT进行认证
- 使用RBAC进行授权

### 10.2 请求限制
- 单用户每分钟最多请求100次
- 关键接口有单独限制

### 10.3 日志记录
- 记录所有API请求
- 记录错误日志

---

**文档版本**: v1.0  
**创建日期**: 2026-06-12  
**适用范围**: 游戏开发团队、程序员、前端开发