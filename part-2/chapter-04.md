---
title: "第 4 章 先 Eval 再开发"
parent: "Part II — 客户发现与 Eval"
nav_order: 2
---

# Chapter 4: 先 Eval 再开发 — 用 Copilot 把评估变成肌肉记忆

## 4.1 没评估 vs 有评估，差的是什么

fde-book 用一张表把差别讲透——差别全在日常对话里：

| 场景 | 没评估的项目 | 有评估的项目 |
|---|---|---|
| 周一站会 | "上周改的那版，感觉还行" | "上周 0.81，这周 0.83，提了 2 个点" |
| 客户问"系统好不好" | 主观打分，每周飘 | 客户每周看数字，可解释 |
| 模型供应商升级 | 不敢动，怕坏 | 一晚上跑一次评估即可决断 |
| 上线 sign-off | 靠演示运气 | 靠数字达成 |
| 项目交接后 | 客户不知道好坏 | 客户能自己跑评估监控 |

最后一行最重要：**评估集不只是开发工具，也是 Handoff 的核心交付物。**

## 4.2 评估驱动的工程闭环

```
改动（prompt / 代码 / 模型）
   ↓
跑评估（机器分 30 秒 + 人工分每周）
   ↓
对比基线（过线？回归？）
   ↓
过线 → 合并 ；未过线 → CI 红灯挡住
   ↓
生产采样 → 失败 case 回流评估集（活水）
```

fde-book 强调四种评估的递进：关键词匹配（第一周够用）→ LLM-as-judge → 配对偏好评估 → agent trajectory 评估（多步推理路径）。**第一周不需要复杂的，30 条 + 关键词匹配就够建立"改动→跑数字→决定上不上"的肌肉记忆。**

## 4.3 落到 Copilot 上（HOW）

这一章是 Ch2 第二条铁律的深化实操。把评估焊进 Copilot 工作流，靠三个工件：

### ① Rule 层：评估是硬约束

`.github/copilot-instructions.md`：

```markdown
## 评估驱动（硬约束）
- 过线分：evals/triage_v0.jsonl，min_score ≥ 0.85
- 任何改动 prompt / 模型 / 检索逻辑的 PR，必须附 `npm run eval` 前后对比。
- 评估回归（分数下降）的 PR，即使功能"看起来对"也禁止合并，除非 PR 描述说明为何可接受。
```

### ② Role 层：`@test-engineer` 兼管评估

扩展 workshop 的 `test-engineer.agent.md`，让它既写单测又跑评估：

```markdown
---
name: test-engineer
description: 写测试并执行 eval-driven 评估对比
---
## 职责
- 为新端点/新功能写 node:test 单测
- 改动涉及 LLM 行为时，执行评估基线对比：
  1. git stash 改动，跑评估记录基线分
  2. 恢复改动，跑评估记录新分
  3. 输出：{基线分, 新分, 差值, 是否过线, 是否回归}
## 输出格式
## Eval Report
- Baseline: 0.81  New: 0.84  Δ +0.03  过线✅  回归❌
```

### ③ CI 层：评估门（来自 workshop ci.yml）

在 `.github/workflows/ci.yml` 里加评估步骤，让"改完代码必须有数字"机械落地：

```yaml
  eval:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - name: Run eval gate
        run: |
          SCORE=$(npm run eval --silent | tail -1)
          echo "eval score: $SCORE"
          # 低于过线分则 exit 1，挡住 PR
          node -e "process.exit($SCORE >= 0.85 ? 0 : 1)"
```

### ④ 从 AWS 到 Azure 的评估栈映射

| 阶段 | fde-book（AWS） | 本书（Azure / Copilot） |
|---|---|---|
| PoC 跑基线 | Bedrock Evaluations 控制台 | Azure AI Foundry Evaluation（控制台版，10 分钟基线） |
| Scaffolding 代码版 | pytest / deepeval | promptflow evals / pytest / deepeval，进 CI |
| RAG 召回评估 | Knowledge Bases Evaluations | Azure AI Search + Foundry RAG 评估指标 |
| Agent 路径评估 | AgentCore Evaluations | Foundry Agent 评估 + trajectory 自定义维度 |
| 线上回流 | CloudWatch 采样 | Azure Monitor / Application Insights 采样 |

> 具体 API 和控制台入口随产品迭代变化，以 learn.microsoft.com 为准。方法论不变：**控制台快出基线 → 代码版进 CI → 线上回流。**

---

## 落到 Copilot 上 · 本章工件清单

- [ ] Rule 层"评估驱动（硬约束）"段
- [ ] `@test-engineer` agent 扩展为兼管评估对比
- [ ] `ci.yml` 评估门（低于过线分 exit 1）
- [ ] 评估栈选定（PoC 用 Foundry 控制台 / 上线用 promptflow + CI）

第二部分到此。客户发现完、评估集建好，进入第三部分：搭工程 Harness 的三层结构。

---

[← 上一章：第一周](chapter-03/) · [下一章：Rule/Role/Workflow 三层 →](../part-3/chapter-05/)
