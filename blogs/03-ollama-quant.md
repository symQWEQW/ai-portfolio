# Ollama 量化部署 qwen2.5

> 政务内网无 GPU 高端卡？量化模型让 7B 也能跑起来。

## 1. 拉取与运行

```bash
ollama pull qwen2.5:7b          # 约 4.7GB（FP16 近似）
ollama pull qwen2.5:7b-q4_K_M   # 量化版，约 4.7GB 实际更省显存
ollama run qwen2.5:7b
```

## 2. 显存 / 内存估算（经验值）

| 版本 | 显存占用 | 适用 |
|---|---|---|
| q4_K_M（4bit） | ~4.7GB | 单卡 6–8G，或纯 CPU 内存 |
| q5_K_M（5bit） | ~5.4GB | 显存 8–12G |
| 原版 7B | ~14GB+ | 16G 以上显卡 |

政务内网常用 16G 显存卡，选 `q4_K_M`/`q5_K_M` 最稳。

## 3. API 调用（Python）

```python
import urllib.request, json
payload = {"model": "qwen2.5:7b", "prompt": "高血压认定标准？", "stream": False}
req = urllib.request.Request(
    "http://localhost:11434/api/generate",
    data=json.dumps(payload).encode(),
    headers={"Content-Type": "application/json"},
)
print(json.loads(urllib.request.urlopen(req).read())["response"])
```

## 4. 为什么是 qwen2.5:7b

- 中文基准领先，政务/医疗问答质量够用
- 支持 function calling / JSON 模式，预审结论可结构化落库
- Ollama 一键部署，与 bge-m3 嵌入同生态，RAG 组合成熟
- 7B 是精度/成本甜蜜点：14B/72B 精度高但部署贵，1.5B/3B 精度不足

> 说明：生成模块在项目里默认 `mock=True`（可离线跑通 RAG 检索与评估），接真实 Ollama 需显式关闭 mock。详见某地项目 [README「模型选型」小节](https://github.com/symQWEQW/jiangling-ai-preaudit/blob/master/README.md) 与 [Dify 私有化部署踩坑实录](https://github.com/symQWEQW/ai-portfolio/blob/master/blogs/01-dify-deploy.md)。
