# 因果修真世界 - 可执行核心规则规格

> **用途**：供程序直接实现的规则清单。与 `SYSTEM_CONSTITUTION.md` 配套。  
> **版本**：v1.0 | 2026-06-12

---

## 一、单命制

```typescript
interface PlayerSoul {
  player_id: string;
  active_life_id: string | null;  // 非 null 时禁止创建新生命
  merit: number;                   // 跨世功德
  sin_karma: Record<string, number>; // 业力路径权重
  soul_archive_id: string;
}
```

**校验点**：
- `POST /life/spawn` → 若 `active_life_id != null` → 403
- 死亡流程完成且选胎成功前 → 不得 spawn

---

## 二、死亡与轮回

### 2.1 正常死亡

```
1. entity.status = dead
2. merit_settle(death_context)
3. judgment = roll_judgment(merit, imprints)  // 仅扩缩池
4. pool = build_reincarnation_pool(merit, judgment)
5. optional: enter_soul_phase(player)         // 可跳过
6. show_reincarnation_ui(pool)
7. player chooses pool_option_id
8. spawn_new_life(pool_option_id)
9. inherit: merit *= 0.3, imprints *= 1.0
10. active_life_id = new_entity_id
```

### 2.2 自杀

```
merit -= 500
clear_memory = true
imprints retain 100%
pool = build_reincarnation_pool(merit_after_penalty, judgment)
// 其余同正常死亡
```

### 2.3 恶意杀害

```
// 无额外 pool_bonus
pool = build_reincarnation_pool(victim_merit, judgment)
// 杀害方通过业力/事件链承担后果，非系统替死者升档
```

---

## 三、投胎池构建

```typescript
function buildReincarnationPool(merit: number, judgment: number): PoolOption[] {
  const baseTiers = meritTierTable(merit);      // 功德定档位上限
  const boundary = judgmentBoundary(judgment);   // 判定值扩缩边界
  return intersect(baseTiers, boundary).filter(o => !o.locked);
}
```

**玩家选择**：`POST /reincarnation/choose` 必须验证 `option_id ∈ pool`。

---

## 四、事件系统（取代任务）

```typescript
interface WorldEvent {
  id: string;
  trigger: StateCondition[];  // 状态变化触发，非 quest_id
  hazard_contribution?: number;
  narrative_hint: string;     // 模糊 UI 文案
  effects: Effect[];
}
```

**禁止**：`Task`, `Quest`, `DailyQuest`, `task_id` 等运行时实体。

---

## 五、因果双轴

| 字段 | 范围 | 可见性 | 用途 |
|------|------|--------|------|
| `merit` | -10000~10000 | 模糊档位 | 投胎池 |
| `sin_karma.path_*` | 0~1 权重 | 不可见 | 事件概率、世界反应 |

**UI 风险文案示例**：
- `可能记恨`
- `后果难料`
- `或将触怒天道`

---

## 六、时间

```
game_years_per_real_day = 100 / 365.25 ≈ 0.274
// 现实 1 年 ≈ 游戏 100 年
```

年龄随 `world_tick` 推进，不使用「1 游戏日 = 1 岁」。

---

## 七、神通与功法

- **神通**：由 `ExperienceRecord` 经感悟规则生成，存入 `abilities.generated[]`
- **功法**：`Item` 类型 `cultivation_method`，从 NPC/遗迹获得
- **禁止**：玩家等级绑定的技能树解锁

---

## 八、战斗（MVP）

- 完整距离分级：贴身 / 短 / 中 / 远 / 超远
- 战斗结果写入 `death_context` 供 hazard 追溯

---

## 九、开发者天道接口（内部）

```
POST /admin/event/inject     // 投事件，不调个体结算
POST /admin/weight/adjust    // 调区域事件权重
POST /admin/apotheosis       // 封神
```

玩家 API **无** 上述路径。

---

**适用范围**：后端、前端、QA
