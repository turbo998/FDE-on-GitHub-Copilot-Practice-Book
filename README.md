# FDE Practice Book — 微软生态下的前置部署工程实践

> **AI 应用落地工程学 · GitHub Copilot / Azure / GitHub 实操版**
>
> 把 FDE 方法论（*为什么 / 什么时候*）焊到 Copilot Harness 工程（*怎么做*）上。

```
Outcome = Harness × Customer
```

**Harness 提供能力，Customer 提供约束。** 这本书写的是：在微软生态（GitHub Copilot + Azure + GitHub）里，怎么把 Harness 真正装到客户身上、跑出客户认账的 Outcome。

---

## 这本书是怎么来的

它是两份材料的**焊接产物**：

| 来源 | 提供什么 | 平台 |
|---|---|---|
| [dawei008/fde-book](https://github.com/dawei008/fde-book) — *OpenBook Vol II · Forward Deployed Engineer* | FDE 方法论：三条铁律、客户发现、Eval-driven、PoC 过线、Agent/Skill/MCP 判断框架 | AWS Bedrock 取材 |
| [turbo998/Harness-Engineering-in-GitHub-Copilot-Workshop](https://github.com/turbo998/Harness-Engineering-in-GithHub-Copilot-Workshop) — *Agent Harness Engineering on Azure* | Copilot Harness 三层模型、10 Agents、6 Prompts、Spec-Kit SDD、Azure 部署、安全沙箱 | GitHub Copilot + Azure |

fde-book 回答 **WHY / WHEN**（取舍判断），workshop 回答 **HOW**（在 Copilot 上的具体 harness 工件）。这本书把两者按"一个真实 FDE 项目"的时间线重新组织：从客户第一周，到 PoC 过线，到 Agent 上线，到交接。

> 致谢与版权见文末。本书文本 CC-BY-SA 4.0，代码 MIT。

---

## 核心论点：两个 Harness 是同一个东西

fde-book 的公式 `Outcome = Harness × Customer` 里的 **Harness**，和 workshop 讲的 **Harness Engineering**，不是巧合同名——它们是同一概念在两个尺度上的展开：

```
       Outcome = Harness × Customer
                    │
        ┌───────────┴───────────┐
        │                       │
   方法论 Harness          工程 Harness
   (fde-book)             (Copilot workshop)
        │                       │
  三条铁律 / Eval-driven    Rule / Role / Workflow 三层
  PoC 过线 / Handoff       instructions / agents / prompts
        │                       │
        └─── 都是"给 AI 套上结构，
              让它在客户约束里做对事" ───┘
```

- **方法论 Harness** 决定*做什么、做到没做到、在哪修*——这是判断层。
- **工程 Harness** 决定*这些判断怎么用 `copilot-instructions.md` / `*.agent.md` / `*.prompt.md` 机械地固化下来*——这是执行层。

**一个 FDE 项目，两层 Harness 都要在。** 只有方法论没有工程，判断落不了地；只有工程没有方法论，做出一堆没人要的 agent。

---

## 目录

| Part | 讲什么 | 焊接的来源 |
|---|---|---|
| **I — 角色与铁律** | FDE 是什么、三条铁律、两层 Harness | fde-book Ch1-3 × workshop DESIGN |
| **II — 客户发现与 Eval** | 第一周、需求→评估集→SOW、先 Eval 再开发 | fde-book Ch4-5,8 × Lab1 |
| **III — Harness 工程化** | Rule/Role/Workflow 三层、把铁律固化成 Copilot 工件 | fde-book Ch6 × Lab2 |
| **IV — 多 Agent 协作** | 手作 handoff → Spec-Kit 工业化 SDD、Agent/MCP/Skill 决策 | fde-book Ch14-16 × Lab3-4 |
| **V — PoC 到生产** | 过线条件、安全沙箱、可观测/成本/灰度/回滚、Azure 部署 | fde-book Ch12-13 × Lab5 + 部署指南 |
| **VI — 交接与精进** | Handoff、模式提取、T 字成长 | fde-book Ch17-18 |
| **附录** | 概念映射表、Copilot Harness 速查、第一周客户启动包、对比矩阵 | 两书附录焊接 |

---

## 怎么读

- **赶时间**：先读 [映射表](appendix/appendix-a-mapping/)——一页看清 FDE 方法论的每个判断对应 Copilot 上哪个工件。
- **带项目读**：每章末尾有「落到 Copilot 上」一节，给可直接 `@mention` / 拷进 `.github/` 的工件。
- **当工具书**：查附录 B（Copilot Harness 速查）、附录 C（第一周启动包）。

---

## 致谢

- **方法论**：[dawei008/fde-book](https://github.com/dawei008/fde-book)，及其引用的 A. Lawrence《FDE Rule Book》、Bob McGrew（"Sell the outcome"）、Nabeel Qureshi《Reflections on Palantir》。
- **工程实践**：[turbo998 的 Copilot Workshop](https://github.com/turbo998/Harness-Engineering-in-GithHub-Copilot-Workshop)，及其参考的 [garrytan/gstack](https://github.com/garrytan/gstack)。

## License

CC-BY-SA 4.0（文本），MIT（代码）。转载、引用、重组都欢迎，请保留来源链接。
