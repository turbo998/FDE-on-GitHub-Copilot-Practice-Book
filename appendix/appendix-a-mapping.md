# 附录 A: 概念映射表 — FDE 方法论 ↔ Copilot 工件

这是全书的中枢索引。一页看清：fde-book 的每个方法论判断，对应 Copilot 工程 Harness 上哪个具体工件。

## A.1 三条铁律 → 工件

| 铁律（方法论 Harness） | Copilot 工件（工程 Harness） | 章节 |
|---|---|---|
| 卖结果不卖产品 | Rule 层 Outcome-First 段 + `@product-reviewer` | Ch2 |
| 评估驱动 | Rule 层过线分 + `@test-engineer` + CI 评估门 + 评估集交付物 | Ch2,4 |
| 在客户那边修（Fix Forward） | Rule 层 Fix-Forward 护栏 + `@investigator` + `scripts/hotfix/` | Ch2 |

## A.2 项目阶段 → 重心工件

| 阶段 | 方法论判断 | 主要工件 | 章节 |
|---|---|---|---|
| Discovery | 对齐 Outcome、建评估集 v0 | `evals/*_v0.jsonl`、`docs/discovery.md` | Ch3 |
| Scaffolding | 技术选型、跑起来 | 三层 harness（Rule→Role→Workflow） | Ch5 |
| Production | 过线、安全、上线 | Spec-Kit、CI 门、安全沙箱、Azure 部署 | Ch6,8 |
| Handoff | 模式提取、教会客户 | 三层交接物全集 | Ch9 |

## A.3 能力扩展 → 形态选择

| 需求 | 形态 | Copilot 落点 | 章节 |
|---|---|---|---|
| 执行确定动作 | Tool | agent 的 tool 定义 | Ch7 |
| 接客户系统 | MCP | Copilot MCP server 接入 | Ch7 |
| 固化方法/规程/手册 | Skill | `skills/<name>/SKILL.md` | Ch7 |
| 全局永远生效的铁律 | Rule | `copilot-instructions.md` | Ch5 |

## A.4 平台映射 — AWS（fde-book）↔ Azure（本书）

| 关注点 | fde-book / AWS | 本书 / 微软生态 |
|---|---|---|
| 开发期 agent 编排 | — | GitHub Copilot agents + Spec-Kit |
| Agent 运行时 | AgentCore + CDK | Azure AI Foundry Agent Service |
| 模型 | Bedrock | Azure OpenAI / Foundry 模型 |
| 评估（PoC） | Bedrock Evaluations | Azure AI Foundry Evaluation |
| 评估（代码版） | pytest/deepeval | promptflow / pytest / deepeval |
| RAG 评估 | Knowledge Bases Evals | Azure AI Search + Foundry RAG 评估 |
| 私网 | VPC | VNet + Private Endpoint |
| 密钥 | Secrets Manager | Azure Key Vault + Managed Identity |
| 可观测 | CloudWatch | Azure Monitor + Application Insights |
| 应用托管 | ECS/Lambda | Azure Container Apps / App Service |
| 灰度回滚 | — | Container Apps revision |
| 成本/清理 | cost + teardown | Azure Cost Management + `azd down` |

## A.5 10 个 Agent → FDE 项目阶段

| Agent | 阶段 | 服务的铁律/判断 |
|---|---|---|
| `@product-reviewer` | Think | 卖结果不卖产品 |
| `@architect` | Think/Plan | 技术选型 |
| `@test-engineer` | Build | 评估驱动 |
| `@doc-writer` | Build | 交接物 |
| `@code-reviewer` | Review | 质量门 |
| `@security-reviewer` | Review | 安全沙箱 |
| `@red-team` | Review | 注入防御 |
| `@performance-engineer` | Review | 成本/性能 |
| `@release-engineer` | Ship | Azure 上线 |
| `@investigator` | 救火 | Fix Forward |

---

[← Ch9](../part-6/chapter-09.md) · [附录 B：Harness 速查 →](appendix-b-harness-cheatsheet.md)
