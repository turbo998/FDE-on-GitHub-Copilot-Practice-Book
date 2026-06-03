---
title: "第 1 章 FDE 与两层 Harness"
parent: "Part I — 角色与铁律"
nav_order: 1
---

# Chapter 1: FDE 是什么 + 两层 Harness

## 1.1 FDE 不是"前线售前 + 后方研发"

合作伙伴的项目经理常把你当成"那个会写代码的售前"。这是误解。FDE（Forward Deployed Engineer，前置部署工程师）的定义是：**在客户现场，同时是售前、是研发、也是运维**。你对客户的业务结果（Outcome）负责，不是对某个功能负责。

在微软生态里，这个角色有个具体形态：你帮合作伙伴或客户把 GitHub Copilot 真正用起来、把 Azure 上的 AI 应用真正跑到生产。你不是来交付一份 PPT 或一个 demo 的，你是来交付一个客户认账的数字的——比如"客服首响时间从 4 小时降到 30 分钟"。

fde-book 把这件事压成一个公式：

```
Outcome = Harness × Customer
```

- **Customer** 是约束：客户的数据、合规、VPC、遗留系统、团队水平——你改不了，只能适配。
- **Harness** 是能力：你给 AI 套上的那套结构，让它在客户的约束里稳定做对事。
- **Outcome** 是两者的乘积——任一项为零，结果就是零。再强的 Harness，碰上一个还没想清楚要什么 Outcome 的客户（Customer 约束里全是模糊），也乘不出东西。

FDE 的工作，就是**调 Harness 这一项**。Customer 那一项你只能理解和适配，调不动。

---

## 1.2 关键洞察：Harness 有两层

这本书最核心的一个观点：**`Outcome = Harness × Customer` 里的 Harness，在落地时分两层，缺一层都不行。**

```
       Harness
          │
   ┌──────┴──────┐
   │             │
方法论 Harness   工程 Harness
（判断层）       （执行层）
   │             │
"做什么/做到没/   "这些判断怎么用
 在哪修"          .github/ 里的文件
                  机械固化下来"
```

### 方法论 Harness（来自 fde-book）

这是**判断层**。它回答三个问题：

- 我现在该做什么？（→ Sell the outcome）
- 我怎么知道做到了？（→ Eval-driven）
- 出问题在哪修？（→ Fix forward）

方法论 Harness 是装在你脑子里的判断标尺。它不产出文件，它产出"决定"。

### 工程 Harness（来自 Copilot workshop）

这是**执行层**。它把上面那些判断，固化成 AI agent 能机械执行的工件。在 GitHub Copilot 里，它是三层结构：

```
┌─────────────────────────────────────────┐
│  Workflow 层   .github/prompts/*.prompt.md │  ← 任务流程：Think→Plan→Build→Test
├─────────────────────────────────────────┤
│  Role 层       .github/agents/*.agent.md   │  ← 专家角色：architect / security-reviewer
├─────────────────────────────────────────┤
│  Rule 层       .github/copilot-instructions.md │ ← 全局铁律：code style / 安全 / 质量门
└─────────────────────────────────────────┘
```

工程 Harness 是装在 git 仓库里的文件。它不做判断，它**忠实执行**判断层定下的规矩——而且是团队每个人、每次对话都一致地执行。

> Harness Engineering 的一句话定义（来自 workshop DESIGN）：**设计 Agent 的工作环境，让它更容易做对事、更难做错事、做错后还能自我修正。**

---

## 1.3 为什么两层都要在

只有一层会怎样？

| 情形 | 后果 |
|---|---|
| 只有方法论 Harness，没有工程 Harness | 你判断得很对（"该先建评估集"），但团队每个人凭记忆执行，下周就走样。判断悬空，落不了地。 |
| 只有工程 Harness，没有方法论 Harness | `.github/` 里塞了 10 个 agent、6 个 prompt，工件很漂亮，但做出一堆客户没要的功能——因为没人判断"这件事服务什么 Outcome"。 |
| 两层都在 | 判断（先建评估集）→ 固化成工件（Rule 层写死过线分，`@test-engineer` 自动跑）→ 团队一致执行。Outcome 可达成。 |

这就是本书每一章的固定结构：**先讲方法论 Harness 的判断（WHY/WHEN），再讲怎么落到工程 Harness 的工件（HOW）。** 每章末尾都有一节「落到 Copilot 上」。

---

## 1.4 一个项目的四个阶段

FDE 项目通常走四个阶段，每个阶段两层 Harness 的重心不同：

| 阶段 | 方法论 Harness 重心 | 工程 Harness 重心 |
|---|---|---|
| **Discovery（发现）** | 对齐 Outcome、建评估集 v0 | 还不写 agent，先把 Rule 层的客户约束记下来 |
| **Scaffolding（脚手架）** | 技术选型、6 周跑起来 | 搭三层 harness：Rule → Role → Workflow |
| **Production（生产）** | PoC 过线、Fix forward | Spec-Kit SDD、CI 门、Azure 部署、安全沙箱 |
| **Handoff（交接）** | 模式提取、把判断教给客户 | 把 `.github/` harness 连同评估集一起交付 |

知道自己在哪个阶段，就知道当前该重点搭哪层 harness。这是后面所有章节的时间线骨架。

---

## 落到 Copilot 上

本章是总览，没有具体工件，但给你一个**项目启动 checklist**——新接一个 Copilot/Azure 落地项目，第一天确认这几件事：

- [ ] 这个项目的 Outcome 数字是什么？（说不出 → 你在 Discovery，先别搭 harness）
- [ ] 客户在哪个平台？（GitHub Enterprise Cloud / Server？Azure 哪个 region？合规要求？）→ 记进 Rule 层草稿
- [ ] 我有没有客户仓库的 push 权限（至少能开 PR）？没有 → 第一周就要谈
- [ ] 客户团队当前怎么用 Copilot？（全员裸用 / 有没有 `copilot-instructions.md`）→ 决定你从哪层 harness 切入

下一章把"方法论 Harness"的三条铁律讲透，并逐条给出它在 Copilot 工件上的固化方式。

---

[下一章：三条铁律 →](chapter-02/)
