<div align="center">

# Specix

**Claude Code 的规范驱动开发插件**

中文 | [English](./README.md)

先定义做什么，再动手实现。用真实证据验证，而不是靠直觉。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

</div>

---

## 为什么需要 Specix？

软件开发中最大的浪费不是写得慢，而是**快速地写错了**。
Specix 结构化你的工作流，确保你解决正确的问题、构建正确的方案、用真实证据验证——而不是靠直觉。

三个核心信念：

- **结构先于行动** — 规范文档是事实来源，不是事后补丁
- **证据先于声明** — "应该没问题"是工程中最危险的词
- **关注点分离** — 不同的问题在不同阶段由不同模块回答

---

## 核心亮点

**规范驱动制品**
每次变更产生四份文档——charter（为什么）、deltas（做什么）、blueprint（怎么做）、lineup（什么时候）。它们是事实来源，不是对话记忆。关掉终端，明天回来——Specix 精确恢复到你离开的地方。

**四层渐进验证**
逐层递进的验证体系，每层回答不同的问题：这个任务按规格实现了吗？代码能跑吗？符合规格且解决了真实问题吗？代码质量、安全性、性能过关吗？

**Git 可选**
14 个模块中 12 个完全不依赖 git。本地开发、原型验证、学习项目——都能享受完整的规范驱动流程。只有 `isolate`（工作区隔离）和 `branch`（集成管理）需要 git。

**可执行阵容**
任务包含精确的文件路径、完整的代码块和可运行的验证命令。拒绝"待定"、"等一下再填"、"类似 X"。每一步都零歧义，可以直接执行。

**跨会话恢复**
状态存在磁盘上的 `.specix/` 目录，不在聊天记录里。一个部分完成的 lineup 就是一个可恢复的检查点——下一个会话读取复选框，继续推进。

**自学习**
`insights.jsonl` 跨会话积累成功模式、踩坑记录和项目特有的惯例。Gateway 每次启动读取这些洞察，路由决策越用越精准。

**反 AI 默认美学**
前端设计模块拒绝千篇一律的 AI 生成模式——紫色渐变、Inter/Roboto 字体、圆角胶囊按钮、"简洁现代"作为目标。每一次生成都必须做出审慎的美学决策。

**双模式触发**
通过模块描述自动匹配自然语言意图，或通过 `/specix:build` 斜杠命令精确调用。用白话描述你想做什么，或者直接按名称调用。

---

## 工作流程

```
想法 ──► spark ──► draft ──► build ──► proof ──► audit ──► lens ──► branch ──► vault
 探索     charter    lineup    执行      验证      审计      评审      集成       归档
          deltas               任务      证据      合规      质量
          blueprint
```

1. **探索** — `specix:spark` 苏格拉底式提问，澄清需求、确认方向
2. **规范** — `specix:draft` 生成 charter、deltas、blueprint、lineup 四份制品
3. **构建** — `specix:build` 验证 lineup 可执行性，通过 `specix:grid` 调度工作者
4. **验证** — `specix:proof` 铁律关卡，跑命令、读输出、要求新鲜证据
5. **审计** — `specix:audit` 双层校验：规格合规 + 真实需求追溯
6. **评审** — `specix:lens` 全局代码质量审查 + 专项透镜
7. **集成** — `specix:branch` 合并、PR、保留或丢弃
8. **归档** — `specix:vault` 将增量合并到主规格，记录经验

---

## 模块概览

**14 个模块**，分布在 4 个层。均以 `specix:` 前缀调用。

### Flow — 流程编排

| 模块 | 用途 |
|------|------|
| [`specix:gateway`](./skills/gateway/SKILL.md) | 会话引导 — 环境检测、路径选择 |
| [`specix:draft`](./skills/flow/draft/SKILL.md) | 生成全部规范制品：charter、deltas、blueprint、lineup |
| [`specix:build`](./skills/flow/build/SKILL.md) | 验证 lineup 可执行性，通过 grid 调度工作者 |
| [`specix:audit`](./skills/flow/audit/SKILL.md) | 双层验证：规格合规 + 真实需求追溯 |
| [`specix:vault`](./skills/flow/vault/SKILL.md) | 归档变更，将 delta 合并到主规格 |

### Craft — 开发工艺

| 模块 | 用途 |
|------|------|
| [`specix:spark`](./skills/craft/spark/SKILL.md) | 探索 + 苏格拉底式构思（轻量模式 / 深度模式） |
| [`specix:probe`](./skills/craft/probe/SKILL.md) | 系统化调试：根因 → 模式 → 假设 → 修复 |
| [`specix:proof`](./skills/craft/proof/SKILL.md) | 铁律关卡 — 跑命令、读输出、确认后才算完成 |

### Engine — 执行引擎

| 模块 | 用途 |
|------|------|
| [`specix:grid`](./skills/engine/grid/SKILL.md) | 多 Agent 并行调度 + 逐任务审查 |
| [`specix:redgreen`](./skills/engine/redgreen/SKILL.md) | 测试驱动：失败测试 → 实现 → 重构 |
| [`specix:lens`](./skills/engine/lens/SKILL.md) | 全局代码审查 + 专项透镜（安全 / 性能 / 架构） |
| [`specix:branch`](./skills/engine/branch/SKILL.md) | 分支生命周期：merge、PR、keep、discard *（需要 git）* |
| [`specix:isolate`](./skills/engine/isolate/SKILL.md) | Git worktree 隔离开发 *（需要 git）* |

### Studio — 领域指导

| 模块 | 用途 |
|------|------|
| [`specix:canvas`](./skills/studio/canvas/SKILL.md) | 前端设计，拒绝千篇一律的 AI 默认美学（按需触发） |

---

## 开发路径

Gateway 启动时检测环境，自动选择合适的路径。

```
用户提出需求
    │
    ▼
[specix:gateway] 评估变更规模
    │
    ├─ 大型变更  ──► 完整路径
    ├─ 中型变更  ──► 精简路径
    ├─ 紧急修复  ──► 修复路径
    └─ 微小改动  ──► 直接执行
```

### 有 Git

```
完整：  spark → draft → isolate → build → proof → audit → lens → branch → vault
精简：  draft → build → proof → audit → lens → branch → vault
修复：  probe → redgreen → proof → (大修复: lens → branch → vault)
```

### 无 Git

```
完整：  spark → draft → build → proof → audit → lens → vault
精简：  draft → build → proof → audit → lens → vault
修复：  probe → redgreen → proof → (大修复: lens → vault)
```

### 独立使用

```
探索：  specix:spark（轻量模式）— 随时可用，纯思考，不产生文件
直接：  直接做 → specix:proof（可选）
```

---

## 工作原理

### 四层验证

每个变更经过渐进式验证——每层回答不同的问题：

| 层级 | 模块 | 回答的问题 |
|------|------|-----------|
| 微观 | `grid` 审查者 | 这个任务按规格正确实现了吗？ |
| 功能 | `proof` | 代码能跑吗？ |
| 语义 | `audit` | 符合规格吗？解决了真实问题吗？ |
| 质量 | `lens` | 代码写得好吗？安全吗？性能好吗？ |

### 规范制品

每次变更在 `.specix/changes/<name>/` 产生四份制品：

```
charter.md      → 为什么做：动机、范围、成功标准
deltas/*.md     → 做什么：增量规格变更（NEW / EDIT / DROP / RENAME）
blueprint.md    → 怎么做：技术设计、关键决策
lineup.md       → 什么时候做：有序任务清单，每步可直接执行
```

### 自学习

Specix 在归档时将观察记录到 `.specix/learn/insights.jsonl`。
随着使用积累哪些模式有效、哪些坑要避免、项目特有的惯例。
Gateway 每次启动读取这些积累，持续改进路由决策。

---

## 安装

### npm（推荐）

```bash
npm install specix
```

### 项目级

克隆或复制到你的项目，然后在 `.claude/settings.json` 中添加：

```json
{ "enabledPlugins": { "specix": true } }
```

### 全局

放置在 `~/.claude/plugins/` 并在用户级设置中启用。

---

## 配置

Specix 读取项目根目录的 `.specix/project.yaml`。首次运行 `specix:draft` 时自动创建。

```yaml
schema: spec-driven

context: |
  Stack: Node.js + TypeScript + React
  Database: PostgreSQL
  Testing: Vitest

rules:
  charter:
    - Include performance impact assessment
  deltas:
    - All endpoints must note auth requirements
  blueprint:
    - Database changes must include migration scripts
  lineup:
    - Each task must include a verification step
```

---

## 模块类型

| 类型 | 含义 | 模块 |
|------|------|------|
| **Iron（铁）** | 刚性。严格遵循。不可绕过。 | `proof`、`probe`、`redgreen`、`grid` |
| **Clay（泥）** | 柔性。灵活适应上下文。 | `spark`、`canvas` |
| **Flow（流）** | 流程。按序执行。 | `draft`、`build`、`audit`、`vault`、`lens`、`branch`、`isolate` |

---

## 许可证

[MIT](./LICENSE)
