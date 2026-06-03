---
title: "第 2 章 三条铁律"
parent: "Part I — 角色与铁律"
nav_order: 2
---

# Chapter 2: 三条铁律，以及它们在 Copilot 上长什么样

方法论 Harness 的内核是三条铁律。它们来自几代 FDE 的经验（Bob McGrew、A. Lawrence、Anthropic/OpenAI 工程实践），fde-book 把它们系统化成一个**有序的判断链**：

```
1. Sell the outcome, not the product   — 决定你做什么
2. Eval-driven                          — 决定你怎么知道做到了
3. Fix forward                          — 决定你在哪里修问题
```

顺序不能换，因为失败代价递减：违反第一条，整个项目方向错（做 12 周没人要的东西）；违反第二条，方向对但不知走到哪；违反第三条，方向对、进度清楚，但救不了客户的火。

这一章逐条讲判断（WHY），并给出**每条铁律在 Copilot 工程 Harness 上的固化点**（HOW）。这是本书相对 fde-book 多出来的一半。

---

## 2.1 第一条：卖结果，不卖产品

### 判断（来自 fde-book）

客户花钱不是想要"一个 Agent"，是想要一个数字：报表准点率 60%→95%、工单首响 4 小时→30 分钟。**Agent 是手段，数字是目的。FDE 对目的负责。**

核心动作是一次**翻译**：把客户嘴里的"我要 X 功能"翻译回"X 功能服务的是什么业务结果"。客户说"能不能也接邮件"，你先问"加邮件是为了解决哪个事——销售跟单回复速度，还是客服首响？"答得上来才排期，答不上来是红灯（这需求是脑补的，做了没人用）。

### 落到 Copilot 上（HOW）

这条铁律是**人的判断**，没法完全机械化——但你能把它固化成 harness 里的两个约束，逼团队每次都做这个翻译：

**① 在 Rule 层写死"Outcome 优先"原则。** `.github/copilot-instructions.md` 里加一段（改自 workshop 的 Core Principles）：

```markdown
## Outcome-First（铁律一）
- 任何新功能的 PR 描述，必须在第一行写明它服务的 Outcome 指标。
  例：`Outcome: 把工单分诊准确率从 0.78 提到 0.85`
- 描述里只写"实现了 X 功能"而没有 Outcome 指标的 PR，reviewer 应打回。
```

**② 用一个 `@product-reviewer` agent 把关。** workshop 自带的 `product-reviewer.agent.md` 角色，正好干这件事——在 Think 阶段从产品视角评审"这个需求服务什么 Outcome、值不值得做"。每个新需求先过它：

```
@product-reviewer 客户想给工单系统加邮件接入。
请从 Outcome 角度评审：这服务哪个业务指标？值得做吗？还是该先确认指标？
```

这样，"把功能翻译回 Outcome"从你一个人的肌肉记忆，变成了团队 harness 里一个强制的 review 关口。

---

## 2.2 第二条：评估驱动

### 判断（来自 fde-book）

LLM 系统和传统软件最大的区别：**输入相同，输出不一定相同；今天能跑的 prompt，模型升级后可能就坏了。** 所以你不能靠"我看了一眼觉得不错"判断好坏。你需要**固定样本集 + 能跑分的脚本**，每次改动都跑一遍。而且是反过来——**先有评估，再有代码**。

五个工程动作：① 第一周就建评估集 v0（20-50 条）；② 每个 PR 跑评估；③ 定一个过线分（写进合同）；④ 生产持续采样回流；⑤ 模型/prompt/依赖大改前先跑基线。

评估集 v0 从 0 到 30 条的冷启动：客户已有数据抽 10 条真实样本 → 自己写 10 条刁难样本 → 让客户业务方看分布挑刺 → 按客户挑刺方向再加 10 条。**关键不是数量，是让客户参与样本选择**——这样交付时他不会说"这评估集本身不对"。

### 落到 Copilot 上（HOW）

这条铁律最适合机械化，工程 Harness 能把它焊死：

**① 过线分写进 Rule 层。**

```markdown
## Eval-Driven（铁律二）
- 本项目过线分：工单分诊评估集 min_score ≥ 0.85（合同 sign-off 指标）。
- 评估集位于 `evals/triage_v0.jsonl`，每个 PR 必须跑 `npm run eval` 并附结果。
- 评估分低于过线分的 PR，CI 红灯，禁止合并。
```

**② 让 `@test-engineer` agent 同时管"测试"和"评估"。** workshop 的 `test-engineer.agent.md` 原本只写测试，扩它的职责：

```
@test-engineer 我刚改了分诊 prompt。请：
1. 跑现状基线评估，记录分数
2. 跑改动后评估
3. 对比两个分数，给出"是否过线 + 是否回归"的结论
```

**③ 评估当 CI 门（来自 workshop 的 ci.yml 思路）。** 在 `.github/workflows/ci.yml` 里加一步，把评估脚本接进 PR 检查——这正是 fde-book 说的"改完代码必须有数字"的机械实现。

**④ 评估集是 Handoff 核心交付物。** 这点 fde-book 和 workshop 一致：客户接手系统时拿到的"是不是工作"的能力，不是你的代码漂不漂亮，是手里有没有评估集 + 跑分脚本。交接章节（Ch9）会再讲。

> **从 AWS 到 Azure 的映射**：fde-book 用 Bedrock Evaluations / Knowledge Bases Evaluations / AgentCore Evaluations。在微软生态里对应：PoC 阶段用 **Azure AI Foundry 的 Evaluation**（控制台 10 分钟跑基线），Scaffolding 后迁到代码版（pytest / promptflow / deepeval，CI 可跑可 diff），上线后用 **Azure Monitor / Application Insights** 收集线上样本回流。

---

## 2.3 第三条：在客户那边修（Fix Forward）

### 判断（来自 fde-book）

Lawrence 反复写的一条：*Don't carry the problem back to HQ. Fix it at the customer site.* 客户对 FDE 的信任，不是建在"问题永远不发生"上，是建在"问题一发生你就在场处理"上。

经典反例：FDE 发现客户数据 5% 是脏的，选择回总部开工单排期，等三周，研发说"通用方案要重构"，第四周客户失去耐心，项目停滞。接手的同事第一天写 30 行 transformer 处理这 5%，第二天数据全过。**教训不是"不要写工单"，是"当场修 + 正式 PR 并行"。**

为了能 Fix Forward，FDE 必须有几样配置，缺一项就退回"实施工程师"：客户环境部署权限（至少 staging）、自己仓库 main 的直接 push 权限（受 CI 保护）、一个 hot fix 上线通道、一个补丁脚本目录。

### 落到 Copilot 上（HOW）

Fix Forward 是关于**权限和通道**的，工程 Harness 在这里的作用是"让现场快修不失控":

**① 安全护栏写进 Rule 层（来自 workshop 的 Safety Guardrails）。** Fix Forward 的风险是"现场手快，改坏生产"。用硬护栏兜底：

```markdown
## Fix-Forward 护栏（铁律三）
- 现场 hot fix 也必须走 PR + CI，禁止直接 force-push main。
- 永不删除文件 / 永不改 .env / credential，除非用户显式确认。
- hot fix PR 标题加 [hotfix] 前缀，必须附"当场救急做了什么 + 正式修复 PR 链接"。
```

**② 用 `@investigator` agent 加速现场归因。** workshop 的 `investigator.agent.md` 专做 root cause 分析。RAG 召回突然掉了，现场第一步：

```
@investigator 工单分诊准确率昨晚从 0.82 掉到 0.61。
请拉 trace、对比昨晚的数据/配置变更，给出最可能的归因和当场可验证的快修方案。
```

**③ 补丁脚本目录纳入 harness。** fde-book 说的"补丁脚本目录"，在 Copilot 项目里就是仓库里一个 `scripts/hotfix/` 目录 + Rule 层一句"这些是当天写、可能一周后扔的脚本，不要求达到正式代码标准，但必须能被 `@investigator` 读懂归因"。

> **权限谈判**：fde-book 强调这四项权限要第一周就争取（拖到第三周对方会觉得"这本不该你有"）。在合作伙伴/客户场景，对应的是 GitHub 仓库的 write 权限、Azure 订阅的 staging 部署权——第一周的客户启动包（附录 C）里要列进去。

---

## 2.4 三条铁律 → 两层 Harness 的合一

把这一章压成一张表，就是本书的中枢：

| 铁律（方法论 Harness 判断） | 固化到的 Copilot 工件（工程 Harness） |
|---|---|
| 卖结果不卖产品 | Rule 层 Outcome-First 段 + `@product-reviewer` 关口 |
| 评估驱动 | Rule 层过线分 + `@test-engineer` 跑评估 + CI 评估门 + 评估集作交付物 |
| 在客户那边修 | Rule 层 Fix-Forward 护栏 + `@investigator` 归因 + `scripts/hotfix/` |

**判断来自 fde-book，工件来自 workshop，焊好就是你能 commit 进客户仓库的东西。** 后面每一章都是这张表某一行的展开。

---

## 收尾

三条铁律不是抽象戒律，是 FDE 每天取舍的判断标尺——而且每一条都能在 Copilot 的 `.github/` 里找到一个固化点。下一部分进入项目时间线的第一站：客户发现与评估集冷启动。

---

[← 上一章：FDE 是什么](chapter-01/) · [下一章：第一周 →](../part-2/chapter-03/)
