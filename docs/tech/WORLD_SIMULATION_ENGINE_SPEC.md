# 因果修真世界 - 世界模拟引擎规格

> **用途**：MVP 世界运行的 Tick + Region 引擎设计。  
> **版本**：v1.0 | 2026-06-12

---

## 一、设计目标

- 单宇宙、无区服：全局一份 `WorldState`
- 事件由状态变化触发，非任务调度
- 支撑人生模拟核心循环，而非 MMO 级并发（Phase 2）

---

## 二、架构概览

```
┌─────────────────────────────────────┐
│           Simulation Loop           │
│  tick() → regions → entities →      │
│  state_diff → event_detector →      │
│  hazard_accumulator → persist       │
└─────────────────────────────────────┘
```

---

## 三、Tick 模型

| 参数 | MVP 值 | 说明 |
|------|--------|------|
| `tick_interval` | 1s 现实时间 | 可配置 |
| `game_time_per_tick` | 按 1:10 换算 | 见生命体验系统 |
| `world_tick` | 单调递增 uint64 | 全局时钟 |

### 3.1 每 Tick 流程

```
1. world_tick++
2. for each active_region:
     update_environment(region)
     for each entity in region:
       update_needs(entity)        // 饥饿、口渴等
       update_age(entity)
       accumulate_hazard(entity)
     detect_state_changes(region)  // diff 触发事件
3. flush_events_to_queue()
4. persist_snapshot(interval=N)
```

---

## 四、Region 分区

```typescript
interface Region {
  id: string;
  bounds: AABB;
  env: EnvironmentState;      // 天气、灵气、危险度
  entities: EntityRef[];
  entropy: number;
}
```

- MVP：静态 Region 网格（如 32×32）
- 玩家实体仅存在于 **一个** Region（单命制）
- Region 环境受世界演化系统缓慢影响

---

## 五、状态机（实体）

```
EntityState:
  alive → dying → dead → (soul_phase) → reincarnating → alive
```

| 状态 | 允许操作 |
|------|----------|
| `alive` | 移动、交互、战斗、修炼 |
| `dying` | 仅只读；触发死亡结算 |
| `dead` | 无实体操作；进入轮回 UI |
| `soul_phase` | 可选短阶段感知 |
| `reincarnating` | 选胎 UI |

---

## 六、事件检测器

```typescript
type StateCondition = {
  field: string;           // e.g. "hunger", "bond.level"
  op: '<' | '>' | '==' | 'changed';
  value: number | string;
};

function detectEvents(before: EntitySnapshot, after: EntitySnapshot): WorldEvent[] {
  return event_rules.filter(r => r.trigger.every(c => eval(c, before, after)));
}
```

**原则**：事件是 **触发器**，不是任务步骤链。

---

## 七、Hazard 累积

```typescript
hazard[entity_id][risk_type] += base_risk * env * merit_mod * tick_delta;

if (hazard_total > threshold) {
  trigger_death(entity, build_cause_chain());
}
```

死因链写入 `DeathContext.cause_chain[]`，供日志与轮回档案。

---

## 八、持久化

| 数据 | 存储 | 频率 |
|------|------|------|
| `WorldState.tick` | Redis | 每 tick |
| `Region.env` | Redis | 每 10 tick |
| `Entity` 核心字段 | PostgreSQL | 状态变更时 |
| `SoulArchive` | PostgreSQL | 死亡时 |

---

## 九、Phase 2 扩展

- Region 分片多进程
- Kafka 事件流
- GPT 驱动的 NPC 决策层（非 MVP）

---

**适用范围**：后端、系统架构
