---
title: "第 7 章 Tool / MCP / Skill"
parent: "Part IV — 多 Agent 协作"
nav_order: 2
---

# Chapter 7: Tool / MCP / Skill — 给 Agent 加能力的三种形态

这是合作伙伴群里最高频的问题之一："我想给 Copilot/Agent 加个能力，到底用 Tool、MCP 还是 Skill？" fde-book 第六部分（Ch16）给了清晰的判断框架，本书把它焊到 Copilot 语境。

## 7.1 三种形态的本质

| 形态 | 是什么 | 类比 |
|---|---|---|
| **Tool** | 一个函数调用，agent 决定何时调 | 给 agent 一个按钮 |
| **MCP** | Model Context Protocol，标准化的能力/数据接入协议 | 给 agent 一个标准插座 |
| **Skill** | 一个目录 + `SKILL.md`，按需加载的"做某类事的方法" | 给 agent 一本可翻阅的操作手册 |

关键区别在**加载时机和 token 成本**：

```
Tool：定义常驻 system prompt，每次对话都占 token
MCP： server 暴露的 tool 列表，连接时注入
Skill：SKILL.md 的 description 常驻（极小），正文按需加载（用到才花 token）
```

## 7.2 Skill 的核心机制：按需加载省 token

fde-book 给的合昇案例很有说服力：把一组操作做成 Skill，**常驻只剩 description（约 1900 token），正文用到才加载，相比全塞进 prompt 的 6000 token，降了约 70%。**

Skill 的结构：

```
my-skill/
├── SKILL.md          # YAML frontmatter: name + description（常驻）
├── references/       # 正文引用的细节（按需加载）
└── scripts/          # 可执行脚本
```

`SKILL.md` 的 frontmatter：

```markdown
---
name: triage-runbook
description: 工单分诊的标准操作流程——分类、优先级、转派规则。当需要处理或审查工单分诊逻辑时加载。
---
# 工单分诊 Runbook
（正文：详细规则、边界 case、客户特定约束……用到才加载）
```

**description 是路由器**：写得好，agent 才知道什么时候该加载这个 skill。description 要写"什么时候用"，不是"是什么"。

## 7.3 FDE 决策框架：什么时候用哪个

fde-book 的判断维度，焊到 Copilot 语境：

| 你的需求 | 选 | 理由 |
|---|---|---|
| 调一个外部 API / 执行一个确定动作 | **Tool** | 单一函数，agent 决定何时调 |
| 接客户已有系统（数据库、内部服务、第三方 SaaS） | **MCP** | 标准协议，一次接入多 agent 复用，生态现成 server |
| 固化"做某类事的方法/规程"，且内容长、不是每次都用 | **Skill** | 按需加载省 token，可版本化交接 |
| 客户的合规/操作手册要喂给 agent | **Skill** | 手册天然是"按需翻阅"，塞进 prompt 浪费 token |

**FDE 现场判断口诀**：
- 是"动作" → Tool
- 是"接系统" → MCP
- 是"方法/规程/手册" → Skill

## 7.4 落到 Copilot 上（HOW）

**① MCP 在 Copilot 里的接入。** GitHub Copilot 支持 MCP server。客户有内部系统要接，优先找现成 MCP server（GitHub、数据库、Azure 资源都有），没有再自己写。接入后 agent 能标准化调用，不用为每个系统写胶水。

**② Skill 在 Copilot 项目里的位置。** Skill 和 Ch5 的三层 harness 互补：
- Rule 层 = 全局永远生效的铁律（常驻）
- Skill = 特定场景才加载的规程（按需）

把客户的"工单分诊 SOP""理赔审核手册""合规检查清单"做成 skill，比全塞进 `copilot-instructions.md` 更省 token、更好维护。

**③ Skill 是优质交接物。** fde-book 强调 skill 可版本化。FDE 交接时，把项目的 skill 目录交给客户——这是"判断如何做事"的显式资产，比口头培训更可靠。

## 7.5 一个反例：不要什么都做成 Skill

fde-book 和工程直觉一致的警告：**Skill 不是银弹。** 一个每次对话都要用的、很短的约束，做成 skill 反而增加加载开销和路由不确定性——它该进 Rule 层常驻。判断标准回到 7.2：**用得频繁且短 → Rule 层；用得偶尔且长 → Skill。**

---

## 落到 Copilot 上 · 本章工件清单

- [ ] 能力需求分类：动作→Tool / 接系统→MCP / 方法手册→Skill
- [ ] 客户内部系统优先找现成 MCP server 接入 Copilot
- [ ] 客户的 SOP/手册做成 `skills/<name>/SKILL.md`（description 写"何时用"）
- [ ] Skill 目录纳入交接清单
- [ ] 频繁且短的约束留在 Rule 层，不做成 Skill

第四部分结束。进入第五部分：PoC 怎么过线、怎么过客户合规、怎么上 Azure 生产。

---

[← 上一章：handoff→SDD](chapter-06/) · [下一章：PoC 到生产 →](../part-5/chapter-08/)
