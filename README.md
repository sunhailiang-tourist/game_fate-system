# 因果修真世界

> 命运无常，因果循环；体验生命，轮回不息

## 项目简介

「因果修真世界」是一款**人生模拟**驱动的修真世界游戏——不是刷等级、做任务的修仙手游，而是让玩家在单条生命中体验生存、选择、衰老与死亡，再以灵魂延续进入下一世。

### 游戏主轴

```
经历 → 感悟 → 神通 → 影响世界 → 轮回
```

修真是可选路径之一，不是默认玩法。

### 核心特色

| 特色 | 描述 |
|------|------|
| **单命制** | 任意时刻仅一条活跃生命，跨世是灵魂延续 |
| **命运与因果** | 功德定投胎池，池内自选；事件驱动，无任务清单 |
| **生命体验** | 生老病死、生存压力、年龄属性变化 |
| **五感共鸣** | 视听嗅味触沉浸感知（MVP 纳入） |
| **情感羁绊** | 跨越轮回的情感与印记传承 |
| **世界演化** | 单宇宙、无区服；玩家行为改变世界 |
| **完整战斗** | MVP 包含距离战斗体系 |

---

## 文档结构

本项目包含17份核心开发文档，分为五大类：

### 一、世界观与背景文档
- `docs/worldview/01_WORLDVIEW_WHITEPAPER.md` - 世界观白皮书
- `docs/worldview/02_SIX_REALMS_SETTINGS.md` - 六界设定集
- `docs/worldview/03_NARRATIVE_OUTLINE.md` - 核心叙事大纲

### 二、核心玩法与机制文档
- `docs/gameplay/01_KARMA_SYSTEM.md` - 因果系统设计
- `docs/gameplay/02_REINCARNATION_SYSTEM.md` - 轮回系统设计
- `docs/gameplay/03_LIFE_EXPERIENCE_SYSTEM.md` - 生命体验系统
- `docs/gameplay/04_SENSE_RESONANCE_SYSTEM.md` - 五感共鸣系统
- `docs/gameplay/05_EMOTIONAL_BOND_SYSTEM.md` - 情感羁绊系统
- `docs/gameplay/06_WORLD_EVOLUTION_SYSTEM.md` - 世界演化系统
- `docs/gameplay/07_CULTIVATION_METHOD_SYSTEM.md` - 功法系统设计

### 三、经济与价值体系文档
- `docs/economy/01_ECONOMIC_SYSTEM.md` - 经济系统设计
- `docs/economy/02_VALUE_SYSTEM.md` - 价值体系设计

### 四、技术架构文档
- `docs/tech/01_SYSTEM_ARCHITECTURE.md` - 系统架构设计
- `docs/tech/02_DATA_STRUCTURE.md` - 数据结构设计
- `docs/tech/03_API_INTERFACE.md` - API接口设计
- `docs/tech/04_TECHNOLOGY_SELECTION.md` - 技术选型文档

### 五、运营与商业化文档
- `docs/operation/01_MONETIZATION_SCHEME.md` - 商业化方案
- `docs/operation/02_OPERATION_STRATEGY.md` - 运营策略

---

## 核心设计理念

### 1. 核心循环
```
生存 → 事件 → 选择 → 因果反馈 → 死亡 → 选胎 → 新一世
```
- 无传统任务系统；「活下去」即是最大挑战
- 事件由状态变化触发，非剧情脚本链

### 2. 命运与轮回
- **单命制**：任意时刻仅一条活跃生命
- **单宇宙、无区服**；出生随机，不可自选
- 功德 + 判定值决定 **可选投胎池** 边界，玩家在池内自选
- 自杀：先 -500 功德，清空记忆、**保留印记**，再在缩减池内自选
- 被恶意杀害不加权补偿，仍按功德定池

### 3. 因果双轴
- **功德**（跨世积累）+ **业力**（当世行为权重）
- UI 模糊风险提示，数值仅后台计算

### 4. 修行与战斗
- 神通由 **经历** 生成，非固定技能树
- 功法为世界物品（遗迹/NPC 传承）
- MVP 包含完整距离战斗体系

### 5. 天道权限
- **言出法随、封神、至高传承** 仅开发者
- 玩家极限为受限子世界，不可言出法随级权限

---

## 技术架构（MVP）

MVP 采用 **Tick + 状态机** 世界模拟；百万并发、GPT 全 NPC 等为 Phase 2。

```
前端 → API → 世界模拟引擎(Tick/Region) → PostgreSQL + Redis
```

详见 `docs/tech/WORLD_SIMULATION_ENGINE_SPEC.md`、`docs/tech/01_SYSTEM_ARCHITECTURE.md`。

---

## 快速开始

### 环境要求
- Node.js >= 20.x
- Go >= 1.22.x
- PostgreSQL >= 16.x
- Redis >= 7.x

### 开发流程
1. 阅读文档目录 `docs/00_DOCUMENT_MAP.md`
2. 深入阅读世界观白皮书 `docs/worldview/01_WORLDVIEW_WHITEPAPER.md`
3. 按模块查阅对应文档

---

## 项目状态

**当前状态**: 文档设计阶段

| 阶段 | 状态 | 预计时间 |
|------|------|----------|
| 文档设计 | ✅ 完成 | 2026-06 |
| 原型开发 | 🔄 规划中 | 2026-07 |
| 测试上线 | ⏳ 待开始 | 2026-12 |

---

## 贡献指南

欢迎开发者、设计师、作家参与项目！

### 贡献方式
- 代码贡献
- 文档完善
- 世界观扩展
- 玩法设计

---

## 联系方式

- 官方论坛: [即将上线]
- Discord: [即将上线]
- 邮箱: contact@fate-realm.com

---

**版权所有**: 因果修真世界开发团队  
### 规格文档（新增）
- `docs/SYSTEM_CONSTITUTION.md` - 开发宪法
- `docs/GAME_CORE_RULES_SPEC.md` - 可执行核心规则
- `docs/tech/WORLD_SIMULATION_ENGINE_SPEC.md` - 世界模拟引擎
- `docs/gameplay/REINCARNATION_UI_SPEC.md` - 选胎界面
- `docs/gameplay/LIFE_CYCLE_BY_FORM_SPEC.md` - 多形态生命

---

**文档版本**: v1.2  
**创建日期**: 2026-06-12  
**更新日期**: 2026-06-12（文档收敛修订）