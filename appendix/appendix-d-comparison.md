---
title: "附录 D 对比矩阵"
parent: "附录"
nav_order: 4
---

# 附录 D: 对比矩阵 — gstack / fde-book / 本书

三份材料的定位、各自补什么、什么时候看哪份。

## D.1 三份材料定位

| 维度 | garrytan/gstack | dawei008/fde-book | 本书（FDE Practice Book） |
|---|---|---|---|
| 性质 | Agent harness 模板仓库 | FDE 方法论书 | 方法论 × 工程的焊接实践书 |
| 回答 | 给我一套现成 agent 配置 | WHY / WHEN（判断） | WHY/WHEN × HOW（判断落到工件） |
| 平台 | 平台中立（配置文件） | AWS Bedrock 取材 | GitHub Copilot + Azure |
| 形态 | 代码 / 配置 | 18 章 + demo | 9 章 + 4 附录 + 工件骨架 |
| 主线 | 无（工具箱） | FDE 项目生命周期 | 一个 Copilot 落地项目时间线 |
| 受众 | 想用 harness 的开发者 | 通用 FDE | 微软生态 FDE / 合作伙伴技术支持 |

## D.2 谁补谁

```
gstack      提供工程哲学（ETHOS、专家 agent、completeness-first）
              ↓ 被 workshop 吸收为 Rule 层核心原则
fde-book    提供方法论判断（三条铁律、Eval-driven、Agent/Skill 决策）
              ↓ 被本书吸收为每章的 WHY/WHEN
workshop    提供 Copilot 具体工件（三层 harness、10 agent、Spec-Kit、Azure 部署）
              ↓ 被本书吸收为每章的 HOW
本书        = fde-book 的判断 焊到 workshop 的工件，按 Copilot 项目时间线重排
```

## D.3 什么时候看哪份

| 你的处境 | 看哪份 |
|---|---|
| 想要一套能直接 fork 的 agent 配置 | gstack |
| 想吃透 FDE 方法论、不限平台（含 AWS 实操） | fde-book |
| 在微软生态给客户/合作伙伴落地 Copilot+Azure | **本书** |
| 客户问"和 Bedrock 比怎样" | 本书附录 A.4 + fde-book 对应章 |
| 要给合作伙伴群发个一页介绍 | 本书 README + 前言 |

## D.4 本书没覆盖、需回原书的部分

本书做了取舍，以下深度内容回 fde-book 原书：

- RAG 工程细节（fde-book Ch7-9：检索、分块、数据管道）
- VPC/网络的 AWS 具体实现（fde-book Ch11，本书只给 Azure 映射）
- Guardrails 的 Bedrock 实现（fde-book Ch13，本书给 Rule 层等价物）
- 四种评估的进阶（LLM-as-judge、配对偏好、trajectory，fde-book 评估章）
- 完整 AgentCore + CDK demo（fde-book demos/ch14-agent）

本书的取舍原则：**判断框架全保留并焊到 Copilot；platform-specific 的 AWS 实操替换成 Azure 映射或指回原书。**

---

[← 附录 C](appendix-c-week1-kit/) · [回到目录](../)
