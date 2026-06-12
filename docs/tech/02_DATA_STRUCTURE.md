# 因果修真世界 - 数据结构设计

## 一、数据模型概述

### 1.1 设计目标
数据结构设计旨在实现：
- **数据完整性**：覆盖所有业务需求
- **数据一致性**：保证数据准确
- **高性能**：支持大规模数据
- **可扩展性**：支持未来扩展

### 1.2 核心实体

| 实体 | 说明 | 存储方式 |
|------|------|----------|
| **Player** | 玩家数据 | PostgreSQL |
| **Entity** | 游戏实体 | PostgreSQL |
| **World** | 世界状态 | Redis |
| **Event** | 事件记录 | Kafka |
| **Inventory** | 物品背包 | PostgreSQL |

---

## 二、核心数据表

### 2.1 Player表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 玩家（灵魂）唯一标识 |
| name | VARCHAR(64) | NOT NULL | 玩家名称 |
| avatar | VARCHAR(256) | | 头像URL |
| active_life_id | UUID | FOREIGN KEY, UNIQUE | 当前唯一活跃生命（单命制） |
| merit | INT | NOT NULL DEFAULT 0 | 功德（跨世） |
| sin_karma | JSONB | NOT NULL DEFAULT '{}' | 业力权重（当世路径参数） |
| gold | BIGINT | NOT NULL DEFAULT 0 | 金币 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |
| updated_at | TIMESTAMP | NOT NULL | 更新时间 |

### 2.2 Entity表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 实体唯一标识 |
| type | VARCHAR(32) | NOT NULL | 实体类型 |
| name | VARCHAR(64) | NOT NULL | 实体名称 |
| karma | INT | NOT NULL DEFAULT 0 | 因果值 |
| merit | INT | NOT NULL DEFAULT 0 | 功德值 |
| region_id | UUID | FOREIGN KEY | 所在区域 |
| status | VARCHAR(32) | NOT NULL | 状态 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |
| updated_at | TIMESTAMP | NOT NULL | 更新时间 |

### 2.3 WorldState表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 状态唯一标识 |
| tick | BIGINT | NOT NULL | 当前Tick |
| entropy | FLOAT | NOT NULL | 世界熵值 |
| region_data | JSONB | NOT NULL | 区域数据 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.4 Event表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 事件唯一标识 |
| type | VARCHAR(32) | NOT NULL | 事件类型 |
| trigger | VARCHAR(256) | NOT NULL | 触发条件 |
| payload | JSONB | NOT NULL | 事件数据 |
| timestamp | BIGINT | NOT NULL | 时间戳 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.5 Inventory表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 物品唯一标识 |
| player_id | UUID | FOREIGN KEY | 所属玩家 |
| item_id | UUID | FOREIGN KEY | 物品类型 |
| quantity | INT | NOT NULL DEFAULT 1 | 数量 |
| position | INT | NOT NULL | 背包位置 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.6 CausalEdge表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 因果边唯一标识 |
| source_id | UUID | FOREIGN KEY | 源节点 |
| target_id | UUID | FOREIGN KEY | 目标节点 |
| delay | BIGINT | NOT NULL | 延迟时间 |
| effect | JSONB | NOT NULL | 影响效果 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.7 SoulArchive表（灵魂档案）

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 档案 ID |
| player_id | UUID | FOREIGN KEY | 灵魂/玩家 ID |
| life_count | INT | NOT NULL | 轮回次数 |
| imprints | JSONB | NOT NULL | 因果印记（100% 继承） |
| merit_carry | INT | NOT NULL | 可继承功德基数 |
| archive_entries | JSONB | NOT NULL | 只读多世摘要 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |
| updated_at | TIMESTAMP | NOT NULL | 更新时间 |

### 2.8 ReincarnationPool表（投胎池快照）

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 池快照 ID |
| player_id | UUID | FOREIGN KEY | 玩家 ID |
| judgment_value | INT | NOT NULL | 判定值（仅扩缩池） |
| merit_at_death | INT | NOT NULL | 死亡时功德 |
| options | JSONB | NOT NULL | 可选投胎项列表 |
| chosen_option_id | UUID | | 玩家选定项 |
| expires_at | TIMESTAMP | NOT NULL | 选胎超时 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.9 ReincarnationRecord表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 记录唯一标识 |
| old_entity | UUID | FOREIGN KEY | 前世实体 |
| new_entity | UUID | FOREIGN KEY | 今生实体 |
| reason | VARCHAR(256) | NOT NULL | 转世原因 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |

### 2.8 Bond表

| 字段名 | 类型 | 约束 | 说明 |
|--------|------|------|------|
| id | UUID | PRIMARY KEY | 羁绊唯一标识 |
| player_id | UUID | FOREIGN KEY | 玩家ID |
| npc_id | UUID | FOREIGN KEY | NPC ID |
| type | VARCHAR(32) | NOT NULL | 羁绊类型 |
| level | INT | NOT NULL DEFAULT 1 | 羁绊等级 |
| affinity | INT | NOT NULL DEFAULT 0 | 好感度 |
| created_at | TIMESTAMP | NOT NULL | 创建时间 |
| updated_at | TIMESTAMP | NOT NULL | 更新时间 |

---

## 三、索引设计

### 3.1 主键索引

| 表名 | 字段 | 索引类型 |
|------|------|----------|
| Player | id | PRIMARY KEY |
| Entity | id | PRIMARY KEY |
| Event | id | PRIMARY KEY |

### 3.2 普通索引

| 表名 | 字段 | 索引类型 | 用途 |
|------|------|----------|------|
| Player | name | UNIQUE | 玩家名称唯一 |
| Player | karma | INDEX | 因果值查询 |
| Entity | region_id | INDEX | 区域查询 |
| Event | type | INDEX | 事件类型查询 |
| Bond | player_id | INDEX | 玩家羁绊查询 |

### 3.3 复合索引

| 表名 | 字段组合 | 索引类型 | 用途 |
|------|----------|----------|------|
| Player | level, exp | COMPOSITE | 等级经验查询 |
| Entity | type, status | COMPOSITE | 类型状态查询 |
| Bond | player_id, npc_id | UNIQUE | 唯一羁绊 |

---

## 四、数据关系

### 4.1 ER图

```
Player 1 ─── * Inventory
Player 1 ─── * Bond
Entity 1 ─── * CausalEdge (source)
Entity 1 ─── * CausalEdge (target)
Entity 1 ─── * ReincarnationRecord (old)
Entity 1 ─── * ReincarnationRecord (new)
```

### 4.2 关系说明

| 关系 | 类型 | 说明 |
|------|------|------|
| Player → Inventory | 1:N | 玩家拥有多个物品 |
| Player → Bond | 1:N | 玩家拥有多个羁绊 |
| Entity → CausalEdge | 1:N | 实体参与多个因果关系 |
| Entity → ReincarnationRecord | 1:N | 实体经历多次转世 |

---

## 五、数据迁移

### 5.1 迁移策略

| 阶段 | 任务 | 时间 |
|------|------|------|
| Phase 1 | 基础表创建 | 1天 |
| Phase 2 | 索引创建 | 1天 |
| Phase 3 | 历史数据迁移 | 2天 |
| Phase 4 | 数据验证 | 1天 |

### 5.2 回滚策略
- 每次迁移前备份数据
- 保留迁移脚本
- 提供回滚脚本

---

## 六、数据安全

### 6.1 加密字段

| 表名 | 字段 | 加密方式 |
|------|------|----------|
| Player | name | AES-256 |
| Player | avatar | URL无需加密 |

### 6.2 访问控制

| 角色 | 权限 | 说明 |
|------|------|------|
| admin | 全部 | 管理员 |
| player | 自己数据 | 玩家只能访问自己的数据 |
| service | 业务数据 | 服务间访问 |

---

## 七、开发注意事项

### 7.1 数据约束
- 使用数据库约束保证数据完整性
- 使用事务保证数据一致性

### 7.2 性能优化
- 使用索引优化查询
- 使用缓存减少数据库压力
- 使用批量操作减少数据库连接

### 7.3 测试要点
- 测试数据完整性
- 测试索引效果
- 测试数据迁移

---

**文档版本**: v1.1  
**创建日期**: 2026-06-12  
**更新日期**: 2026-06-12（active_life_id、灵魂档案、投胎池）  
**适用范围**: 游戏开发团队、程序员、DBA