# 因果修真世界 - 游戏开发文档目录

## 项目概述
「因果修真世界」是一款 **人生模拟** 驱动的修真世界游戏，核心特色包括：
- 单命制与功德门槛自选投胎
- 真实的生命体验（生老病死、生存压力）
- 事件驱动（无任务清单）
- 五感共鸣的沉浸体验（MVP）
- 经历生神通，非技能树成长
- 单宇宙、无区服

### 游戏主轴
```
经历 → 感悟 → 神通 → 影响世界 → 轮回
```

---

## 文档目录结构

### 〇、宪法与核心规格（优先阅读）
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 0.1 | `/docs/SYSTEM_CONSTITUTION.md` | 开发宪法 | ✅ |
| 0.2 | `/docs/GAME_CORE_RULES_SPEC.md` | 可执行核心规则 | ✅ |
| 0.3 | `/docs/REVISION_CHECKLIST.md` | 文档修订清单 | ✅ 已收敛 |

### 一、世界观与背景文档
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 1.1 | `/docs/worldview/01_WORLDVIEW_WHITEPAPER.md` | 世界观白皮书 | ✅ v1.1 |
| 1.2 | `/docs/worldview/02_SIX_REALMS_SETTINGS.md` | 六界设定集 | ✅ |
| 1.3 | `/docs/worldview/03_NARRATIVE_OUTLINE.md` | 核心叙事大纲 | ✅ |

### 二、核心玩法与机制文档
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 2.1 | `/docs/gameplay/01_KARMA_SYSTEM.md` | 因果系统设计 | ✅ v1.1 |
| 2.2 | `/docs/gameplay/02_REINCARNATION_SYSTEM.md` | 轮回系统设计 | ✅ v1.2 |
| 2.3 | `/docs/gameplay/03_LIFE_EXPERIENCE_SYSTEM.md` | 生命体验系统 | ✅ v1.2 |
| 2.4 | `/docs/gameplay/04_SENSE_RESONANCE_SYSTEM.md` | 五感共鸣系统 | ✅ MVP |
| 2.5 | `/docs/gameplay/05_EMOTIONAL_BOND_SYSTEM.md` | 情感羁绊系统 | ✅ |
| 2.6 | `/docs/gameplay/06_WORLD_EVOLUTION_SYSTEM.md` | 世界演化系统 | ✅ |
| 2.7 | `/docs/gameplay/07_CULTIVATION_METHOD_SYSTEM.md` | 功法系统设计 | ✅ v1.1 |
| 2.8 | `/docs/gameplay/08_RANDOM_EVENT_SYSTEM.md` | 随机事件系统 | ✅ v1.1 |
| 2.9 | `/docs/gameplay/09_WORLD_INITIALIZATION_SYSTEM.md` | 世界初始化系统 | ✅ |
| 2.10 | `/docs/gameplay/10_CULTIVATION_REALM_SYSTEM.md` | 境界体系系统 | ✅ |
| 2.11 | `/docs/gameplay/11_RESOURCE_SYSTEM.md` | 资源系统设计 | ✅ |
| 2.12 | `/docs/gameplay/12_COMBAT_SYSTEM.md` | 战斗系统设计 | ✅ v1.1 MVP |
| 2.13 | `/docs/gameplay/REINCARNATION_UI_SPEC.md` | 选胎界面规格 | ✅ 新增 |
| 2.14 | `/docs/gameplay/LIFE_CYCLE_BY_FORM_SPEC.md` | 多形态生命规格 | ✅ 新增 |

### 三、经济与价值体系文档
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 3.1 | `/docs/economy/01_ECONOMIC_SYSTEM.md` | 经济系统设计 | ✅ |
| 3.2 | `/docs/economy/02_VALUE_SYSTEM.md` | 价值体系设计 | ✅ |

### 四、技术架构文档
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 4.1 | `/docs/tech/01_SYSTEM_ARCHITECTURE.md` | 系统架构设计 | ✅ MVP |
| 4.2 | `/docs/tech/02_DATA_STRUCTURE.md` | 数据结构设计 | ✅ v1.1 |
| 4.3 | `/docs/tech/03_API_INTERFACE.md` | API接口设计 | ✅ v1.1 |
| 4.4 | `/docs/tech/04_TECHNOLOGY_SELECTION.md` | 技术选型文档 | ✅ |
| 4.5 | `/docs/tech/WORLD_SIMULATION_ENGINE_SPEC.md` | 世界模拟引擎 | ✅ 新增 |

### 五、运营与商业化文档（非 MVP）
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 5.1 | `/docs/operation/01_MONETIZATION_SCHEME.md` | 商业化方案 | ⏸ 非 MVP |
| 5.2 | `/docs/operation/02_OPERATION_STRATEGY.md` | 运营策略 | ⏸ 非 MVP |

### 六、参考文档
| 序号 | 文件路径 | 文档名称 | 状态 |
|------|----------|----------|------|
| 6.1 | `/docs/reference/01_CHATGPT_DESIGN_CONVERSATION.md` | ChatGPT 设计对话记录 | ✅ 溯源 |

---

## 文档阅读指南

### 开发者入门路径
1. `SYSTEM_CONSTITUTION.md` → `GAME_CORE_RULES_SPEC.md`
2. 世界观白皮书 → 生命体验 + 轮回 + 因果
3. `WORLD_SIMULATION_ENGINE_SPEC.md` → 技术架构
4. 按模块查阅 gameplay 文档

### 文档规范
- **格式**：Markdown格式，支持目录跳转
- **术语**：单命制、投胎池、功德、业力、无任务
- **版本**：核心文档已标注 v1.1+

---

## 版本历史
| 版本 | 日期 | 更新内容 | 作者 |
|------|------|----------|------|
| v1.0 | 2026-06-12 | 初始版本，完成所有核心文档 | System |
| v1.1 | 2026-06-12 | 单命制、功德门槛自选投胎；导入设计对话与修订清单 | System |
| v1.2 | 2026-06-12 | 全量文档收敛：宪法、核心规则、Tick引擎、选胎UI、多形态；废除任务API | System |

---

**文档总数：31份**
