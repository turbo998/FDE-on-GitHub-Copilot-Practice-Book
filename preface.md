# 前言：为什么要把两本书焊在一起

## 一个真实的星期一

你是给微软合作伙伴做技术落地的工程师。周一早上，合作伙伴群里有人 @你：

> "我们想用 GitHub Copilot 给团队提效，也想在 Azure 上把那个客服 Agent 跑起来。Copilot 我们买了 Enterprise，但用下来就是个'高级补全'，团队每个人写出来的东西还是参差不齐。Agent 那边在 demo 上能跑，一进客户的合规和 VPC 就卡死。你能不能帮我们理一下，到底该怎么落地？"

这个问题有两半，市面上的材料各答一半：

- **FDE 方法论的书**（比如 dawei008 的 *fde-book*）答得了"该做什么、做到没做到怎么衡量、PoC 怎么过线"——但它的动手 demo 跑在 AWS Bedrock 上，你在微软生态里照搬不了。
- **Copilot 的 workshop**（比如你自己做的那份）答得了"`copilot-instructions.md` 怎么写、10 个 agent 怎么分工、Spec-Kit 怎么跑 SDD"——但它是教学材料，缺一条"在真实客户项目里，这些工件什么时候该上、按什么判断上"的主线。

这本书把这两半焊在一起。

---

## 焊接的逻辑：WHY/WHEN × HOW

两份材料的分工特别干净：

```
fde-book           →  WHY / WHEN   （取舍判断：做什么、何时做、做到没做到）
Copilot workshop   →  HOW          （具体工件：instructions / agents / prompts / Spec-Kit）
```

举个最直接的例子。fde-book 第二条铁律说"Eval-driven，先有评估集再写代码"。这是 **WHY**——你信了，但你在 Copilot 项目里具体怎么落？

这本书的对应章节会告诉你 **HOW**：把"过线分"写进 `.github/copilot-instructions.md` 的 Rule 层，让 `@test-engineer` agent 每个 PR 自动跑评估，让 CI 在低于阈值时挡住合并。判断来自 fde-book，工件来自 workshop，焊好就是你周一能用的东西。

---

## 这本书不重复的事

- 不重抄 fde-book 的全部 18 章——只取**判断框架**，platform-specific 的 AWS demo 替换成 Azure/Copilot 对应物。
- 不重抄 workshop 的全部 5 个 Lab——只取**可复用工件**，并补上每个工件"什么时候该用"的判断主线。
- 不讲 Copilot 基础操作（补全、Chat 面板）——假设你已经会，直接进工程。

## 写给谁

- 给微软合作伙伴 / 客户做 Copilot 与 Azure AI 落地的工程师
- 已经会用 Copilot，但想从"个人提效"升级到"团队级 / 客户级 harness 工程"的人
- 要把一次性的 Agent demo 变成能过客户合规、能上 Azure 生产的人

如果你完全没碰过客户现场、没用过 Copilot Agent，建议先跑一遍原始的 [Copilot Workshop](https://github.com/turbo998/Harness-Engineering-in-GithHub-Copilot-Workshop) 再回来。

---

## 三个承诺（继承自 fde-book）

1. **每个判断都有一个具体场景**——不写"FDE 要重视评估"这种空话。
2. **每个技术选型都给决策维度**——不写"Agent 比 RAG 好"，给你一组在客户语境里自己判断的维度。
3. **每个判断都落到一个 Copilot 工件**——这是本书相对 fde-book 多出来的承诺：判断不悬空，落到你 `.github/` 目录里能 commit 的文件。

> *焊接于 2026-06，微软合作伙伴技术支持视角。*
