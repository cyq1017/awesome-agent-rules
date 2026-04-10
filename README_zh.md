# Awesome Agent Rules [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | **中文**

> 精选的 AI 编程 Agent 规则、指令和最佳实践合集。
>
> CLAUDE.md · AGENTS.md · .cursorrules · GEMINI.md — 来自真实项目的实战经验。

每个 AI 编程工具都会读取某种指令文件，但**大多数开发者每次都从零开始写**。这个仓库收集了经过验证的规则模式，让你不必重复造轮子。

---

## 为什么这很重要

AI 编程 Agent（Claude Code、Cursor、Windsurf、Codex、Antigravity 等）很强大——但它们需要约束。没有好的规则：

- 🔥 Agent 会覆盖整个文件而不是局部修改
- 🏃 Agent 还没理解需求就开始写代码
- 💀 断连后上下文全部丢失
- 🤷 Agent 反复犯同样的错

好的规则能解决这一切。**一个 `.md` 文件就能改变你的 AI 编程体验。**

---

## 目录

- [各工具对应的指令文件](#各工具对应的指令文件)
- [规则分类](#规则分类)
  - [任务管理](#-任务管理)
  - [会话交接](#-会话交接)
  - [执行纪律](#-执行纪律)
  - [错误追踪](#-错误追踪)
  - [信任校准](#-信任校准)
  - [多 Agent 协作](#-多-agent-协作)
  - [工具选择](#-工具选择)
  - [网络容错](#-网络容错)
- [即用模板](#即用模板)
- [相关项目](#相关项目)
- [贡献指南](#贡献指南)

---

## 各工具对应的指令文件

| 工具 | 文件 | 作用范围 |
|------|------|---------|
| **Claude Code** | `CLAUDE.md`（项目）/ `~/.claude/CLAUDE.md`（全局） | 单项目或全局 |
| **Cursor** | `.cursorrules` | 单项目 |
| **Windsurf** | `.windsurfrules` | 单项目 |
| **Codex (OpenAI)** | `AGENTS.md` / `~/.codex/AGENTS.md` | 单项目或全局 |
| **Antigravity** | `AGENTS.md` / `~/.gemini/AGENTS.md` | 单项目或全局 |
| **Aider** | `.aider.conf.yml` | 单项目 |
| **多 Agent 通用** | `AGENTS.md` | 跨工具标准 |

> 💡 **AGENTS.md** 是新兴的跨工具标准 — Claude Code、Codex、Antigravity 都能读取。如果你只维护一个文件，就选 `AGENTS.md`。

---

## 规则分类

### 📋 任务管理

**最常见的错误：Agent 还没理解需求就开始写代码。**

```markdown
## 任务启动协议

收到新任务后，不要立即编码。先评估并回复：

1. 我理解的目标是：____
2. 预估涉及文件数：____
3. 预估耗时：____
4. 建议任务等级：S / M / L
5. 不确定的地方：____

等用户确认后再开始。

### 任务等级 → 流程对应

| 等级 | 标准 | 流程 |
|------|------|------|
| **S** | < 30分钟，1-2个文件 | 直接做 → 测试 → 提交 → HANDOFF |
| **M** | 1-3小时，3-5个文件 | 简要计划 → 执行 → 提交 → HANDOFF |
| **L** | > 3小时，跨模块 | 头脑风暴 → 计划 → TDD → 审查 → HANDOFF |
```

**为什么有效：** 强制在执行前对齐理解，避免"我以为你要 X 但你其实要 Y"的问题。

---

### 🔄 会话交接

**第二常见的错误：上下文在 session 之间丢失。**

```markdown
## 会话交接协议

### 开始时
1. 读取 `HANDOFF.md` — 了解上次做了什么
2. 运行 `git log --oneline -5` 确认最近提交
3. 检查 `git status` 是否有未提交的改动
4. 确认理解后再开始工作

### 结束时（必须执行）
更新 `HANDOFF.md`，格式如下：

## YYYY-MM-DD
- done：[完成事项]
- blocked：[阻塞项，无则省略]
- decisions：[关键决策及理由]
- pitfalls：[踩坑记录]
- next：[建议下一步]

控制在 500 token 以内。记录所有否定决策（尝试了什么、为什么放弃）。
```

**为什么有效：** HANDOFF.md 是最便宜、最可靠的跨 session 记忆。适用于任何工具，不怕崩溃，人也能读。

---

### 🎯 渐进式 HANDOFF

**不要等 session 结束才写 HANDOFF——你可能随时断连。**

```markdown
## 渐进式 HANDOFF 规则

不要等 session 结束才写 HANDOFF。每完成一个里程碑就更新：
- 每次 git commit 后 → 追加到 Done 列表
- 做了重要决策后 → 追加到 Decisions
- 踩坑后 → 追加到 Pitfalls
- 这样即使 session 意外中断，HANDOFF 也包含最新状态
```

**为什么有效：** 网络断连、崩溃、上下文耗尽都很常见。渐进式 HANDOFF 确保你最多只丢失一步的进度。

---

### 🛡️ 执行纪律

```markdown
## 执行纪律

- 修改已有文件用局部替换，绝不覆盖整个文件
- 严格按用户确认的计划执行——不跳步，不做额外优化
- 分析/判断完成后先展示，等用户确认再执行
- 说了要做的事必须立刻写入文件
- 不假设数据不会丢——修改重要文件前先备份
- 每完成一个大阶段后主动建议压缩/总结上下文
```

**为什么有效：** Agent 天生热心而且倾向于多做。这些规则防止范围蔓延和数据丢失。

---

### 📝 错误追踪

```markdown
## 错误追踪

犯错时：
1. 立即承认——不要试图掩盖
2. 配合人类的错误记录流程（AI 错题本）
3. 本次 session 内不要重复同样的错误
4. 不确定的事情要问，不要猜

## 错误分类（用于错题本）
给每个错误打标签：
- `幻觉` — 声称了虚假信息
- `抢跑` — 未确认就开始执行
- `API 错误` — API 用法错误
- `流程违规` — 跳过了必要步骤
- `越界` — 做了未被要求的工作
```

**为什么有效：** 系统化的错误追踪长期构建出"Agent 信任画像"。你会知道哪个 Agent 在什么任务上靠谱。

---

### 🤝 信任校准

```markdown
## 信任校准框架

并非所有任务都需要相同程度的监督：

| 信任层级 | 适用场景 | 人类行动 |
|---------|---------|---------|
| **L1 — 验结果** | 你不懂过程，但能检查产出 | 跑测试、检查部署 |
| **L2 — 交叉验证** | 用另一个 Agent 审查 | 让 Agent B 审 Agent A 的代码 |
| **L3 — 小范围试** | 先在小范围验证，再推广 | 先在一个文件上试方案 |
| **L4 — 追问原因** | 你不理解决策背后的原因 | 问"为什么选这个方案？备选方案呢？" |
```

**为什么有效：** 信任不是二元的。这些层级帮你判断什么时候该仔细审查、什么时候可以放手。

---

### 🤖 多 Agent 协作

```markdown
## 多 Agent 交接

### 多个 Agent 操作同一个项目时
- 每个 Agent 开工前必须读取 HANDOFF.md——没有例外
- 每个 Agent 结束后必须更新 HANDOFF.md——没有例外
- 冲突的决策必须标记 ⚠️，留给人类裁决
- 未经用户明确许可，不得覆盖另一个 Agent 的工作

### 断连恢复协议
当用户说"断开了"/"继续之前的工作"/"做到哪了"时：
1. 读取 HANDOFF.md（最可靠的恢复点）
2. `git log --oneline -5` 确认最后提交
3. `git status` 检查未提交改动
4. 汇报恢复结果——不要搜索无关文件
```

---

### 🔧 工具选择

```markdown
## 工具选择规则

- 验证类任务（检查文件、仓库、状态）→ 用命令行，不开浏览器
- 浏览器只用于必须交互的操作（登录、填表单、点按钮）
- 浏览器子代理卡住超过 30 秒 → 立即放弃，换命令行方案
- "看一眼效果"这类需求 → 给用户链接，不替用户开浏览器
```

---

### 🌐 网络容错

```markdown
## 网络容错

- 长时间操作前先 `git commit`
- 不稳定连接要设置重试和超时机制
- 包管理器 (pip, npm, cargo) 走代理容易超时
  → 配置国内镜像或 DIRECT 路由规则
- AI 服务域名必须走纯净住宅 IP 路由
```

---

## 即用模板

开箱即用的模板，适用于不同类型的项目：

| 模板 | 适用场景 | 下载 |
|------|---------|------|
| [精简版](templates/minimal.md) | 快速脚本、小项目 | [📄](templates/minimal.md) |
| [标准版](templates/standard.md) | 大多数项目 | [📄](templates/standard.md) |
| [多 Agent 版](templates/multi-agent.md) | 多个 AI Agent 协作的项目 | [📄](templates/multi-agent.md) |

---

## 相关项目

| 项目 | 说明 |
|------|------|
| [Conductor](https://github.com/cyq1017/conductor) | 多 AI 编程 Agent 协作治理的 CLI 框架 |
| [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | Cursor 专用规则合集 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | AI Agent 的 Spec 驱动开发框架 |
| [Superpowers (ECC)](https://github.com/obra/superpowers) | Claude Code 的 Agent 工作流增强技能集 |
| [AGENTS.md 规范](https://agents.md) | AGENTS.md 跨工具标准的官方规范 |
| [claude-code (instructkr)](https://github.com/instructkr/claude-code) | Claude Code 技巧和 CLAUDE.md 示例 |

---

## 贡献指南

欢迎贡献！请先阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。

**如何贡献：**
1. Fork 本仓库
2. 在对应分类中添加你的规则/模板
3. **必须脱敏所有个人数据**（路径、用户名、API key）
4. 提交 PR 时说明：
   - 用哪个工具测试的
   - 解决了什么问题
   - 使用了多长时间

---

## 许可证

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

在法律允许的范围内，贡献者已放弃对本作品的所有版权和相关权利。
