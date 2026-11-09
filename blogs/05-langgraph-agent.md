# LangGraph 多 Agent 编排

> 目标 JD 对齐：楚天云「大模型算法研发工程师」明确要求 *LangChain/LangGraph/Spring AI，Dify 优先*。

## 1. 单链不够用时，上多 Agent

某地 AI 预审若用单条提示词链，病种识别、政策检索、合规审核全挤在一次调用里，
**难优化、难观测、难扩展**。改成三个 Agent 协作：

```
患者问题 → Planner（识别病种/拆解材料）
                 ↓
           Retriever（按病种查政策）
                 ↓
           Auditor（综合出结论）
```

## 2. LangGraph 代码骨架

```python
from langgraph.graph import StateGraph, END
g = StateGraph(State)
g.add_node("planner", planner)
g.add_node("retriever", retriever)
g.add_node("auditor", auditor)
g.set_entry_point("planner")
g.add_edge("planner", "retriever")
g.add_edge("retriever", "auditor")
g.add_edge("auditor", END)
app = g.compile()
```

## 3. 多 Agent 的工程价值

- **职责解耦**：检索 Agent 可单独换成 RAG 服务，不影响其他节点
- **可观测**：每步输入输出可记录、评估、重试
- **可扩展**：加「申诉 Agent」「人工复核 Agent」复用同一张图
- **可控**：比 Dify 拖拽更适合复杂政务合规场景

未安装 `langgraph` 时，项目内置顺序执行回退模式，逻辑完全一致，可零依赖演示。

## 4. 当前边界（如实说明）

这是**本地回环的编排演示**：三个 Agent 的逻辑为 mock，尚未接入真实 LLM 与政策库，
**没有上线、也没有接入生产预审链路**。它的价值在架构设计与可替换性，不在业务闭环。

流程图与各 Agent 职责见仓库 [`docs/flow.md`](https://github.com/symQWEQW/preaudit-multi-agent/blob/master/docs/flow.md)，
完整实现见 [`preaudit-multi-agent`](https://github.com/symQWEQW/preaudit-multi-agent)。
