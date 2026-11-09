# 🧭 AI 应用落地工程师 · 作品集与技术博客

> 统一叙事「AI 应用落地工程师」——聚合某地 AI 预审（RAG）、LangGraph 多智能体、医疗 LoRA 微调三个项目，并沉淀 5 篇可公开的技术博客。

![Role](https://img.shields.io/badge/Role-AI%20应用落地工程师-blue)
![Blogs](https://img.shields.io/badge/Blogs-5%20篇-green)
![License](https://img.shields.io/badge/License-MIT-green)

> **网名：无**（公开内容不署真实姓名）
> 定位：**AI 应用落地工程师**——把大模型变成能用的政务/医疗产品。
> 下面三个仓库是从「会用 Dify」走向「懂 AI 工程」的递进式作品。
> 代码仓库：GitHub [@symQWEQW](https://github.com/symQWEQW)。

**👉 在线作品集（GitHub Pages）：** 本仓库 `index.html` 位于根目录，推上 `master` 后由 `.github/workflows/pages.yml` 自动部署；本地直接双击 `index.html` 也能打开。

---

## 统一叙事：从应用到原理

```
             会用平台 ──────▶ 懂 AI 工程 ──────▶ 碰过模型
                 │                  │                  │
    ┌────────────┴──────┐  ┌────────┴───────┐  ┌──────┴─────────┐
    │ preaudit-         │  │ preaudit-      │  │ medical-       │
    │ rag-engine        │  │ multi-agent    │  │ lora-finetune  │
    │ RAG 应用 + 评估    │  │ LangGraph 编排 │  │ LoRA 微调+评估  │
    └───────────────────┘  └────────────────┘  └────────────────┘
```

## 三个仓库

| 仓库 | 角色 | 技术栈 | 解决什么 | 关键实证 |
|---|---|---|---|---|
| [preaudit-rag-engine](https://github.com/symQWEQW/preaudit-rag-engine) | AI 工程作品（能讲深） | TF-IDF / BM25 + Ollama + RAG 评估 | 切片策略、重排序、量化评估 | 16 篇 / 13,390 字语料，38 题，HitRate@5 **0.974** / Recall@5 **0.549** / MRR@5 **0.715** |
| [preaudit-multi-agent](https://github.com/symQWEQW/preaudit-multi-agent) | 多 Agent 编排 | LangGraph / StateGraph | Planner / Retriever / Auditor 三 Agent 协作 | 无依赖顺序回退，本地回环演示（**未上线 / 未接生产**） |
| [medical-lora-finetune](https://github.com/symQWEQW/medical-lora-finetune) | 原理级证据 | LoRA / PEFT / PyTorch | 训练并评估模型，不止调 API | 字符级 bigram F1 **0.065 → 0.150（+130.3%）**，26 条训练样本，CPU 3.5 min |

> 仓库公开地址（均为绝对链接）：
> - https://github.com/symQWEQW/preaudit-rag-engine
> - https://github.com/symQWEQW/preaudit-multi-agent
> - https://github.com/symQWEQW/medical-lora-finetune

### ★ 最值得讲的一件事

**发现并撤回了被语料规模撑起来的满分读数。** 旧评测「HitRate@5 = 1.000 / Recall@5 = 1.000」不是模型好，而是 5 篇语料切成 5 个 chunk、计分窗口恰好也是 top-5 —— top-5 ⊇ 全部候选，满分在数学上必然。重建为 16 篇 / 13,390 字 + 4 篇干扰、38 题、k=3/5/10 三档后，真实读数是 **0.974 / 0.549 / 0.715**。完整复算证据见 [RESULTS.md](https://github.com/symQWEQW/preaudit-rag-engine/blob/master/RESULTS.md) 第四节。

---

## 技术博客

均在 [`blogs/`](https://github.com/symQWEQW/ai-portfolio/tree/master/blogs)，统一用绝对 GitHub 链接（可渲染 Markdown）：

1. [Dify 私有化部署踩坑实录](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/01-dify-deploy.md)
2. [RAG 召回调优：从指标到实验（v2 重写）](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/02-rag-tuning.md)
3. [Ollama 量化部署与显存优化](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/03-ollama-quant.md)
4. [LoRA 微调入门：让小模型懂医疗问答](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/04-lora-intro.md)
5. [LangGraph 多智能体协作编排](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/05-langgraph-agent.md)

## 页面里的图

`diagrams/` 存放从 `archify-output/` 现成图表导出的真实渲染图：

| 文件 | 用途 |
|---|---|
| `preaudit-architecture.png` / `.html` / `.json` | 系统架构：组件、边界、AI 审核边界 |
| `preaudit-workflow.png` / `.html` / `.json` | 业务流程：填报 → 审核 → 通知 → 窗口预审 |

PNG 由 HTML 无头渲染导出（1300×1320），交互原图与源 JSON 一并保留可核对；**没有手绘示意图，也没有编造截图**。

## 关于我

- 实习：某信息化交付公司，参与某地政务慢特病 AI 预审、医疗信息化软著、智能客服测试与数据治理
- 技能：Spring Boot / Vue3 / Dify / Ollama / RAG / Python / Docker / PEFT-LoRA / LangGraph
- 关注方向：AI 应用落地（RAG / 微调 / 多智能体）+ 政企医疗场景交付

## 数据口径与脱敏

- **所有数字都带出处**：主项目指标来自 [RESULTS.md](https://github.com/symQWEQW/preaudit-rag-engine/blob/master/RESULTS.md)（`run_eval.py` 现场跑出，明细 `data/eval_result_v2.json`）；LoRA 指标来自 medical-lora-finetune 的 `README.md` 与 `outputs/eval_result.txt`（本机 CPU 实测）。未做四舍五入美化。
- **局限如实写**：主项目生成模块仍为 `mock=True` 默认、rerank 与召回同源；LoRA 仅 26 条样本、F1 绝对值 0.150 偏低、远未达上线水平；多智能体仅本地回环，**三个项目均未声称上线生产**。
- ⚠️ 所有项目数据均为**模拟**，不含真实患者信息；地理名称已统一脱敏为「某地」。公开前请逐条核对 [脱敏清单.md](https://github.com/symQWEQW/ai-portfolio/blob/master/%E8%84%B1%E6%95%8F%E6%B8%85%E5%8D%95.md)。
- 仓库已统一使用 `preaudit-*` 命名，不含地理名称。

## 本地预览 / 部署

```bash
# 本地：直接打开根目录 index.html，或
python -m http.server 8000
# 打开 http://localhost:8000

# 上线：推到 master/main 即触发 .github/workflows/pages.yml
# 首次需在仓库 Settings → Pages → Build and deployment 选择 "GitHub Actions"
```

## License

MIT
