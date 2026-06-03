---
title: "第 6 章 handoff → Spec-Kit"
parent: "Part IV — 多 Agent 协作"
nav_order: 1
---

# Chapter 6: 手作 handoff → Spec-Kit 工业化 SDD

## 6.1 多 Agent 协作的两个段位

fde-book 讲 Agent 时反复强调：**单 Agent 能解决的问题，别上多 Agent**——多 Agent 的协调成本很高。但当任务确实需要多个专家视角（写代码 + 安全审查 + 测试 + 发版），协作就不可避免。

workshop 把多 Agent 协作分成两个段位，正好是 FDE 项目成熟度的两个阶段：

```
段位一：手作 handoff（Lab3）
  你手动 @architect → 拿结果 → @code-reviewer → @security-reviewer ...
  理解协作怎么发生，但靠人串联

段位二：工业化 SDD（Lab4，Spec-Kit）
  /specify → /plan → /tasks → /implement
  规格驱动，agent 按 spec 自动协作，可复现
```

FDE 的判断：**PoC 阶段用段位一**（灵活、你看得见每步），**生产化阶段升段位二**（可复现、可交接、团队规模化）。

## 6.2 段位一：手作 handoff（来自 Lab3）

多 agent 协作的本质是**结构化的 handoff**——一个 agent 的输出成为下一个 agent 的输入。workshop 的协作协议模板（`collaboration-protocol-template.md`）规定了 handoff 的格式，关键是每次交接都带：

- 上一个 agent 做了什么（done）
- 验证了什么（verified）
- 还剩什么 / 交给谁（next）

这其实就是第一章「检查点」原则的多 agent 版。一个典型的手作链：

```
1. @architect      设计工单分诊的模块边界 → 产出设计文档
2. @code-reviewer  审查实现 → 产出 review 报告
3. @security-reviewer  查注入/越权 → 产出安全报告
4. @test-engineer  写测试 + 跑评估 → 产出 eval report
5. @release-engineer  发版 + changelog
```

**FDE 现场价值**：手作 handoff 让你和客户**看得见每个专家视角的判断**。客户问"你们怎么保证安全"，你直接给 `@security-reviewer` 的结构化报告。这种透明度在建立信任阶段（Discovery/PoC）比自动化更值钱。

## 6.3 段位二：Spec-Kit 工业化 SDD（来自 Lab4）

当 PoC 过线、要规模化时，手作 handoff 的问题暴露：**不可复现、依赖你在场串联。** Spec-Kit（GitHub 的 Spec-Driven Development 工具）把协作工业化：

```
/specify   写规格——要做什么、验收标准（含评估过线分！）
   ↓
/plan      技术方案——架构、依赖、风险
   ↓
/tasks     拆任务——可并行的工作单元
   ↓
/implement  agent 按 spec 自动协作实现
```

关键焊接点：**`/specify` 阶段的验收标准，就是 Ch3 的评估集 + 过线分。** Spec-Kit 的 spec 不是空泛的"实现工单分诊"，而是"实现工单分诊，验收 = evals/triage_v0.jsonl 上 min_score ≥ 0.85"。方法论 Harness 的判断（评估驱动）直接进了 SDD 流程的入口。

**FDE 判断：什么时候从手作升到 Spec-Kit？**

| 信号 | 还留在手作 | 该升 Spec-Kit |
|---|---|---|
| 团队规模 | 你一个人 / 2 人 | 3+ 人，需要分工不撞车 |
| 复现需求 | PoC，一次性 | 生产，要重复交付类似功能 |
| 交接压力 | 还没到交接 | 客户团队要接手自己开发 |
| 功能复杂度 | 单一端点 | 多模块、跨服务 |

## 6.4 从 AWS 到微软生态的映射

fde-book 的 Agent 章（Ch14）基于 **AWS AgentCore + CDK**。本书的多 agent 协作完全在 **GitHub Copilot + Spec-Kit** 上，不依赖 AWS。如果客户的 Agent 运行时要上 Azure：

- 编排/协作层：GitHub Copilot agents + Spec-Kit（开发期）
- 运行时层：Azure AI Foundry Agent Service（生产期多 agent 运行时）
- 二者衔接：开发期用 Spec-Kit 把 agent 行为定义清楚、评估过线，再部署到 Foundry Agent Service 运行

---

## 落到 Copilot 上 · 本章工件清单

- [ ] `docs/collaboration-protocol.md` — handoff 格式（done/verified/next）
- [ ] PoC 阶段：跑通一条手作 handoff 链并存档（给客户看透明度）
- [ ] 生产阶段：装 Spec-Kit，`/specify` 里写入评估过线分作验收
- [ ] 升级判断 checklist（团队规模 / 复现 / 交接 / 复杂度）

下一章解决一个高频客户问题：给 agent 加能力，到底用 Tool、MCP 还是 Skill？

---

[← 上一章：三层 harness](../part-3/chapter-05/) · [下一章：Tool/MCP/Skill →](chapter-07/)
