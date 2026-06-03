---
title: 首页
nav_order: 0
description: "FDE on GitHub Copilot · Practice Book — 微软生态下的前置部署工程实践"
permalink: /
---

# FDE on GitHub Copilot · Practice Book
{: .fs-9 }

微软生态下的前置部署工程实践 — 把 FDE 方法论焊到 GitHub Copilot Harness 工程上。
{: .fs-6 .fw-300 }

<p align="center">
  <img src="{{ '/assets/images/cover.png' | relative_url }}" alt="FDE on GitHub Copilot · 实践之书 封面 — 超级机器人大战风格群像" width="420" style="max-width:100%;border-radius:12px;box-shadow:0 8px 32px rgba(9,105,218,0.35);margin:1.5rem auto;">
</p>

[开始阅读](preface){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[概念映射表](appendix/appendix-a-mapping){: .btn .fs-5 .mb-4 .mb-md-0 .mr-2 }
[GitHub 仓库](https://github.com/turbo998/FDE-on-GitHub-Copilot-Practice-Book){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## 这本书是什么

它是两份材料的**焊接产物**：[dawei008/fde-book](https://github.com/dawei008/fde-book) 提供 FDE 方法论判断（WHY / WHEN），[Copilot Workshop](https://github.com/turbo998/Harness-Engineering-in-GithHub-Copilot-Workshop) 提供 GitHub Copilot 上的具体工程工件（HOW）。本书按一个真实 Copilot / Azure 落地项目的时间线把两者重新组织。

公式：

> **Outcome = Harness × Customer**
>
> Harness 提供能力，Customer 提供约束。FDE 的工作是把 Harness 装到 Customer 身上、跑出客户认账的 Outcome。

而 Harness 分两层：**方法论 Harness**（判断层，来自 fde-book）+ **工程 Harness**（执行层，来自 Copilot workshop）。一个 FDE 项目，两层都要在。

---

## 怎么读

- **赶时间**：先看 [附录 A 概念映射表](appendix/appendix-a-mapping) —— 一页看清每个 FDE 判断对应 Copilot 上哪个工件。
- **带项目读**：每章末尾「落到 Copilot 上」给可直接 commit 进客户仓库的工件。
- **当工具书**：查 [附录 B Harness 速查](appendix/appendix-b-harness-cheatsheet)、[附录 C 第一周启动包](appendix/appendix-c-week1-kit)。

---

## 目录

**Part I — 角色与铁律** · [FDE 与两层 Harness](part-1/chapter-01) · [三条铁律](part-1/chapter-02)

**Part II — 客户发现与 Eval** · [第一周](part-2/chapter-03) · [先 Eval 再开发](part-2/chapter-04)

**Part III — Harness 工程化** · [Rule/Role/Workflow 三层](part-3/chapter-05)

**Part IV — 多 Agent 协作** · [handoff→Spec-Kit](part-4/chapter-06) · [Tool/MCP/Skill](part-4/chapter-07)

**Part V — PoC 到生产** · [过线+安全沙箱+Azure 上线](part-5/chapter-08)

**Part VI — 交接与精进** · [Handoff 与 T 字成长](part-6/chapter-09)

**附录** · [A 映射表](appendix/appendix-a-mapping) · [B Harness 速查](appendix/appendix-b-harness-cheatsheet) · [C 第一周启动包](appendix/appendix-c-week1-kit) · [D 对比矩阵](appendix/appendix-d-comparison)

---

致谢：[dawei008/fde-book](https://github.com/dawei008/fde-book)（方法论）、[garrytan/gstack](https://github.com/garrytan/gstack)（工程哲学）。文本 CC-BY-SA 4.0，代码 MIT。
