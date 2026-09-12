# 🧭 AI 应用落地工程师 · 作品集与技术博客

> 统一叙事「AI 应用落地工程师」——聚合某地 AI 预审、医疗 LoRA 微调、LangGraph 多智能体三大项目，并沉淀 5 篇可公开的技术博客。

![Role](https://img.shields.io/badge/Role-AI%20应用落地工程师-blue)
![Blogs](https://img.shields.io/badge/Blogs-5%20篇-green)
![License](https://img.shields.io/badge/License-MIT-green)

---

> **网名：无**（公开内容不署真实姓名）
> 实习方向信息化交付。
> 定位：**AI 应用落地工程师**——把大模型变成能用的政务/医疗产品。
> 下面三个仓库是我从「会用 Dify」走向「懂 AI 工程」的递进式作品。
>
> 代码仓库：GitHub @symQWEQW。

## 统一叙事：从应用到原理

```
            会用平台 ──────▶ 懂 AI 工程 ──────▶ 碰过模型
                │                  │                  │
   ┌────────────┴──────┐  ┌────────┴───────┐  ┌──────┴─────────┐
   │ preaudit-        │  │ preaudit-     │  │ medical-       │
   │ ai-preaudit       │  │ multi-agent    │  │ lora-finetune  │
   │ RAG 应用+评估     │  │ LangGraph 编排 │  │ LoRA 微调+评估  │
   └───────────────────┘  └────────────────┘  └────────────────┘
```

## 三大仓库

| 仓库 | 角色 | 技术栈 | 解决什么 |
|---|---|---|---|
| [preaudit-rag-engine](../../preaudit-rag-engine) | AI 工程作品（能讲深） | TF-IDF/Milvus + Ollama + RAG 评估 | 切片策略、重排序、量化评估 |
| [preaudit-multi-agent](../../preaudit-multi-agent) | 多 Agent 编排（命中楚天云 JD） | LangGraph | 规划/检索/审核三 Agent 协作 |
| [medical-lora-finetune](../../medical-lora-finetune) | 原理级证据 | LoRA/PEFT | 训练并评估模型，不止调 API |

> 仓库公开地址：
> - preaudit-rag-engine: https://github.com/symQWEQW/preaudit-rag-engine
> - preaudit-multi-agent: https://github.com/symQWEQW/preaudit-multi-agent
> - medical-lora-finetune: https://github.com/symQWEQW/medical-lora-finetune

## 技术博客

见 [`blogs/`](./blogs)：
1. [Dify 私有化部署踩坑](./blogs/01-dify-deploy.md)
2. [RAG 召回调优：切片与重排序](./blogs/02-rag-tuning.md)
3. [Ollama 量化部署 qwen2.5](./blogs/03-ollama-quant.md)
4. [LoRA 微调入门：让小模型懂医疗问答](./blogs/04-lora-intro.md)
5. [LangGraph 多 Agent 编排](./blogs/05-langgraph-agent.md)

## 关于我

- 实习：某信息化交付公司（信息化交付），参与某地政务慢特病 AI 预审、医疗信息化软著、智能客服测试与数据治理
- 技能：Spring Boot / Vue3 / Dify / Ollama / RAG(Milvus) / Python / Docker
- 关注方向：AI 应用落地（RAG / 微调 / 多智能体）+ 政企医疗场景交付

> ⚠️ 所有项目数据均为**模拟**，不含真实患者信息；公开前请逐条核对脱敏清单。
