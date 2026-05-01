<div align="center">

# ⬡ Specix

**Claude Code 的规范驱动开发插件**

中文 | [English](./README.md)

**先定义做什么，再动手实现。用真实证据验证，而不是靠直觉。**

```bash
claude plugin marketplace add devnomad-byte/specix
claude plugin install specix@devnomad-byte-specix
```

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![模块](https://img.shields.io/badge/modules-14-blueviolet.svg)](./skills/)
[![验证层](https://img.shields.io/badge/verification_layers-4-9cf.svg)](./README_CN.md#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)

</div>

---

## 🎯 为什么需要 Specix？

> 软件开发中最大的浪费不是写得慢，而是**快速地写错了**。

三个核心信念：

- 🏗️ **结构先于行动** — 规范文档是事实来源，不是事后补丁
- 🔬 **证据先于声明** — "应该没问题"是工程中最危险的词
- 🧩 **关注点分离** — 不同的问题在不同阶段由不同模块回答

---

## ✨ 核心亮点

> 📋 **规范驱动制品**
> 每次变更产生四份文档——charter（为什么）、deltas（做什么）、blueprint（怎么做）、lineup（什么时候）。
> 事实来源存在磁盘上，不在对话记忆里。

> 🔍 **四层渐进验证**
> 微观 → 功能 → 语义 → 质量。每层回答不同的问题。

> 🔓 **Git 可选**
> 14 个模块中 12 个完全不依赖 git。本地开发、原型验证、学习项目——完整流程，无需仓库。

> 🎯 **可执行阵容**
> 精确的文件路径、完整的代码块、可运行的验证命令。零歧义，零占位符。

> 💾 **跨会话恢复**
> 状态在磁盘上，不在聊天里。关掉终端，明天回来，精确恢复到离开的地方。

> 🧠 **自学习**
> `insights.jsonl` 跨会话积累成功模式和踩坑记录。越用越智能。

> 🎨 **反 AI 默认美学**
> 前端设计模块拒绝千篇一律——紫色渐变、Inter/Roboto、圆角胶囊按钮。只接受审慎的美学决策。

> ⚡ **双模式触发**
> 自然语言自动匹配，或 `/specix:build` 斜杠命令精确调用。随你选。

---

## 🔄 工作流程

```
                        ┌──────────────────────────────────────────────────────────────────────┐
                        │                       Specix 完整管线                                │
                        └──────────────────────────────────────────────────────────────────────┘

   ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
   │         │     │         │     │         │     │         │     │         │     │         │     │         │     │         │
   │  spark  │────►│  draft  │────►│  build  │────►│  proof  │────►│  audit  │────►│  lens   │────►│ branch  │────►│  vault  │
   │         │     │         │     │         │     │         │     │         │     │         │     │         │     │         │
   └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘
     探索            规范             执行             验证             审计             评审             集成             归档
   苏格拉底       四份制品         grid调度         新鲜证据        规格+需求        代码质量        merge/PR       delta合并
   轻量+深度     charter/deltas   工作者+审查       跑命令          追溯            专项透镜        生命周期       自学习
                  bluep./lineup                    读输出          三级回退                         insights
```

---

## 📦 模块概览

**14 个模块**，分布在 **4 个层**。均以 `specix:` 前缀调用。

### 🌊 Flow — 流程编排

| 模块 | 类型 | 用途 |
|------|:----:|------|
| [`specix:gateway`](./skills/gateway/SKILL.md) | Flow | 会话引导 — 环境检测、路径选择 |
| [`specix:draft`](./skills/flow/draft/SKILL.md) | Flow | 生成全部规范制品：charter、deltas、blueprint、lineup |
| [`specix:build`](./skills/flow/build/SKILL.md) | Flow | 验证 lineup 可执行性，通过 grid 调度工作者 |
| [`specix:audit`](./skills/flow/audit/SKILL.md) | Flow | 双层验证：规格合规 + 真实需求追溯 |
| [`specix:vault`](./skills/flow/vault/SKILL.md) | Flow | 归档变更，将 delta 合并到主规格 |

### 🔨 Craft — 开发工艺

| 模块 | 类型 | 用途 |
|------|:----:|------|
| [`specix:spark`](./skills/craft/spark/SKILL.md) | Clay | 探索 + 苏格拉底式构思（轻量模式 / 深度模式） |
| [`specix:probe`](./skills/craft/probe/SKILL.md) | Iron | 系统化调试：根因 → 模式 → 假设 → 修复 |
| [`specix:proof`](./skills/craft/proof/SKILL.md) | Iron | 铁律关卡 — 跑命令、读输出、确认后才算完成 |

### ⚡ Engine — 执行引擎

| 模块 | 类型 | 用途 |
|------|:----:|------|
| [`specix:grid`](./skills/engine/grid/SKILL.md) | Iron | 多 Agent 并行调度 + 逐任务审查 |
| [`specix:redgreen`](./skills/engine/redgreen/SKILL.md) | Iron | 测试驱动：失败测试 → 实现 → 重构 |
| [`specix:lens`](./skills/engine/lens/SKILL.md) | Flow | 全局代码审查 + 专项透镜（安全 / 性能 / 架构） |
| [`specix:branch`](./skills/engine/branch/SKILL.md) | Flow | 分支生命周期：merge、PR、keep、discard *（需要 git）* |
| [`specix:isolate`](./skills/engine/isolate/SKILL.md) | Flow | Git worktree 隔离开发 *（需要 git）* |

### 🎨 Studio — 领域指导

| 模块 | 类型 | 用途 |
|------|:----:|------|
| [`specix:canvas`](./skills/studio/canvas/SKILL.md) | Clay | 前端设计，拒绝千篇一律的 AI 默认美学（按需触发） |

### 模块类型

| 类型 | 图标 | 含义 | 模块 |
|------|:----:|------|------|
| **Iron（铁）** | 🛡️ | 刚性，严格遵循，不可绕过 | `proof` `probe` `redgreen` `grid` |
| **Clay（泥）** | 🏺 | 柔性，灵活适应上下文 | `spark` `canvas` |
| **Flow（流）** | 🌊 | 流程，按序执行 | `draft` `build` `audit` `vault` `lens` `branch` `isolate` |

---

## 🛤️ 开发路径

Gateway 启动时检测环境，自动选择合适的路径。

```
用户提出需求
    │
    ▼
[specix:gateway] 评估变更规模
    │
    ├─ 🔴 大型变更  ──► 完整路径
    ├─ 🟡 中型变更  ──► 精简路径
    ├─ 🟠 紧急修复  ──► 修复路径
    └─ 🟢 微小改动  ──► 直接执行
```

### 有 Git

```bash
完整：  spark → draft → isolate → build → proof → audit → lens → branch → vault
精简：  draft → build → proof → audit → lens → branch → vault
修复：  probe → redgreen → proof → (大修复: lens → branch → vault)
```

### 无 Git

```bash
完整：  spark → draft → build → proof → audit → lens → vault
精简：  draft → build → proof → audit → lens → vault
修复：  probe → redgreen → proof → (大修复: lens → vault)
```

### 独立使用

```bash
探索：  specix:spark（轻量模式）— 随时可用，纯思考，不产生文件
直接：  直接做 → specix:proof（可选）
```

---

## ⚙️ 工作原理

### 🔍 四层验证

每个变更经过渐进式验证——每层回答不同的问题：

```
  ┌─────────────────────────────────────────────────────┐
  │  质量       │  lens        │  代码写得好吗？安全吗？  │
  ├─────────────────────────────────────────────────────┤
  │  语义       │  audit       │  符合规格吗？解决了真实  │
  │             │              │  问题吗？                │
  ├─────────────────────────────────────────────────────┤
  │  功能       │  proof       │  代码能跑吗？            │
  ├─────────────────────────────────────────────────────┤
  │  微观       │  grid 审查者  │  按规格正确实现了吗？    │
  └─────────────────────────────────────────────────────┘
```

### 📋 规范制品

每次变更在 `.specix/changes/<name>/` 产生四份制品：

```
charter.md      → 为什么做：动机、范围、成功标准
deltas/*.md     → 做什么：增量规格变更（NEW / EDIT / DROP / RENAME）
blueprint.md    → 怎么做：技术设计、关键决策
lineup.md       → 什么时候做：有序任务清单，每步可直接执行
```

### 🧠 自学习

Specix 在归档时将观察记录到 `.specix/learn/insights.jsonl`。
随着使用积累哪些模式有效、哪些坑要避免、项目特有的惯例。
Gateway 每次启动读取这些积累，持续改进路由决策。

---

## 📥 安装

### 在 Claude Code 会话中

```
/plugin marketplace add devnomad-byte/specix
/plugin install specix@devnomad-byte-specix
```

### 在终端中

```bash
claude plugin marketplace add devnomad-byte/specix
claude plugin install specix@devnomad-byte-specix
```

### 安装范围

| 范围 | 效果 |
|------|------|
| `--scope user` 🏠 | 所有项目可用（默认） |
| `--scope project` 👥 | 通过 `.claude/settings.json` 与协作者共享 |
| `--scope local` 🔒 | 仅在当前仓库中可用 |

### 手动安装

克隆仓库并添加到 `.claude/settings.json`：

```json
{ "enabledPlugins": { "specix": true } }
```

---

## 🔧 配置

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

<div align="center">

[MIT License](./LICENSE) · Made with ⬡ for Claude Code

</div>
