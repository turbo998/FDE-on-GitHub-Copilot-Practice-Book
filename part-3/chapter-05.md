---
title: "第 5 章 Rule/Role/Workflow 三层"
parent: "Part III — Harness 工程化"
nav_order: 1
---

# Chapter 5: Rule / Role / Workflow 三层 — 把铁律固化成 Copilot 工件

这一章是工程 Harness 的核心施工图。前面几章反复说"落到工件上"，这一章把那个"工件系统"完整搭起来。

## 5.1 为什么是三层

workshop 的 DESIGN 给了一个关键对比：**Prompt Engineering 是技能，Harness Engineering 是工程学科。**

| 维度 | Prompt Engineering | Harness Engineering |
|---|---|---|
| 作用范围 | 单次对话 | 整个项目生命周期 |
| 可复用性 | 低，靠个人经验 | 高，团队共享配置文件 |
| 一致性 | 每次对话可能不同 | 规则层保证行为一致 |
| 安全性 | 靠人记 | 系统级护栏强制 |
| 协作 | 单人 | 多 agent 分工 |
| 可审计 | 无 | 规则和流程可 review、可版本控制 |

FDE 在客户现场最怕的就是"团队每个人用 Copilot 用出不同结果"。三层 harness 就是治这个的：

```
┌─────────────────────────────────────────┐
│  Workflow 层  .github/prompts/*.prompt.md │  编排任务流程
├─────────────────────────────────────────┤
│  Role 层      .github/agents/*.agent.md   │  定义专家角色
├─────────────────────────────────────────┤
│  Rule 层      .github/copilot-instructions.md │ 全局铁律（地基）
└─────────────────────────────────────────┘
```

**自底向上强制**：Rule 层是地基，所有 agent 和 workflow 都必须遵守；Role 层定义专家视角；Workflow 层编排具体任务。

## 5.2 Rule 层 — copilot-instructions.md

这是地基，也是三条铁律的固化点（Ch2 已分段给过，这里给完整骨架）。一个客户项目的 Rule 层应该包含：

```markdown
# Project Conventions

## 语言与风格
- 技术栈约定（语言、框架、测试框架）—— 跟客户现有代码库一致，不另起炉灶

## 三条铁律
### Outcome-First
- PR 第一行写明服务的 Outcome 指标
### 评估驱动
- 过线分 + 每个 PR 跑评估 + CI 门
### Fix-Forward 护栏
- hot fix 也走 PR+CI；永不 force-push main / 删文件 / 改 .env

## 安全护栏（硬约束，绝不违反）
- ❌ 永不删除文件（除非显式确认）
- ❌ 永不 force-push main/master
- ❌ 永不改 .env / credential
- ❌ API 响应永不暴露 stack trace

## 核心原则（改自 gstack ETHOS）
1. Completeness First — 完整的错误处理/输入校验/边界，半成品比没有更糟
2. Search Before Building — 先看现有模式再造新抽象，复用 > 重造
3. User Sovereignty — 建议但不覆盖用户的显式决定

## 协作协议
- 每个 agent 有明确角色边界，不越界
- 不确定时问，别猜
- 需要别的专长时 @mention 对应 agent
```

**FDE 关键点**：Rule 层必须**贴客户的约束写**，不是抄模板。客户用 Java 你就别写 JavaScript 约定；客户有等保合规你就把数据流约束写进去。这是 `Outcome = Harness × Customer` 里 Customer 那一项在 harness 里的投影。

## 5.3 Role 层 — *.agent.md

每个 agent = 一个专家视角（gstack 哲学）。workshop 给了 10 个现成的，按 FDE 项目阶段分组：

| 阶段 | 该上的 agent | 干什么 |
|---|---|---|
| Think/Discovery | `@product-reviewer` `@architect` | 评审 Outcome、定架构 |
| Build | `@test-engineer` `@doc-writer` | 写测试+评估、写文档 |
| Review | `@code-reviewer` `@security-reviewer` `@red-team` `@performance-engineer` | 多维审查 |
| Ship | `@release-engineer` | 发版、changelog |
| 现场救火 | `@investigator` | root cause 归因 |

一个 agent 文件的形状（security-reviewer 为例）：

```markdown
---
name: security-reviewer
description: API 代码安全审查专家
---
# Security Reviewer Agent
你是审查 Node.js Express API 的安全专家。
## 职责
- 常见漏洞、输入校验完整性、注入攻击、错误处理（无 stack trace 泄露）
## 规则
- 绝不直接改代码
- 输出结构化报告，分级 CRITICAL / WARNING / INFO
- 每条给：位置、问题、建议
```

**FDE 渐进式采用建议（来自 workshop 企业最佳实践）**：别一上来塞 10 个 agent。
- 第 1 周：只配 Rule 层
- 第 2 周：加 2-3 个核心 agent（通常 `@code-reviewer` `@test-engineer` `@security-reviewer`）
- 第 3 周：加 Workflow 层 prompt

## 5.4 Workflow 层 — *.prompt.md

把多步任务编排成可复用模板，可以 `@mention` 具体 agent 执行子任务。workshop 给了 6 个：`add-endpoint` / `fix-bug` / `code-review` / `design-feature` / `investigate-issue` / `ship-release`。

一个 prompt 文件就是把 Sprint 流程（Think→Plan→Build→Review→Test→Ship）的某一段固化。例如 `code-review.prompt.md` 一键拉起多角色联合审查（Ch6 详讲）。

## 5.5 三层 harness 全部 version control

workshop 企业最佳实践里最该划重点的一条，也是 FDE 交接的命脉：

> **把 `copilot-instructions.md`、`*.agent.md`、`*.prompt.md` 全部纳入 Git，改规则走 PR review，不私自改。**

这条等于把方法论 Harness（判断）也变成了可审计、可交接的资产。客户接手时，拿到的不只是代码，是**整套带判断逻辑的 harness**。

---

## 落到 Copilot 上 · 本章工件清单

- [ ] `.github/copilot-instructions.md` — 贴客户约束的 Rule 层
- [ ] `.github/agents/` — 按阶段渐进上线的 agent（先 3 个）
- [ ] `.github/prompts/` — Workflow 层模板
- [ ] 全部纳入 git，改规则走 PR
- [ ] 渐进式采用：周1 Rule → 周2 Role → 周3 Workflow

第三部分搭完三层地基。第四部分讲这些 agent 怎么**协作**——从手作 handoff 到工业化 SDD。

---

[← 上一章：先 Eval 再开发](../part-2/chapter-04/) · [下一章：多 Agent 协作 →](../part-4/chapter-06/)
