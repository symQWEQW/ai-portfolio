# Dify 私有化部署踩坑记录

> 适用：政务/医疗内网场景，需要把 Dify + Ollama 跑在本地，数据不出域。

## 1. 一键起 Dify

```bash
git clone https://github.com/langgenius/dify.git
cd dify/docker
cp .env.example .env
docker compose up -d
```

访问 `http://localhost`（Nginx 80 端口）。首次访问 `/console/api/setup` 完成管理员初始化。

## 2. 接入本地 Ollama（最容易踩的坑）

Ollama 监听 `11434`，但 Dify 容器在 docker 网络中，**不能直接用 `localhost`**。

```bash
# 在 .env 中把模型供应商地址设为宿主可达地址
OLLAMA_API_BASE=http://host.docker.internal:11434
```

Windows / macOS 的 Docker Desktop 默认支持 `host.docker.internal`；Linux 需额外加 `--add-host=host.docker.internal:host-gateway`。

## 3. 模型「调不通 / 超时」排查 SOP

| 现象 | 可能原因 | 处理 |
|---|---|---|
| 模型列表拉不到 | Ollama 未起 / 地址错 | `ollama list` 确认；检查 `host.docker.internal` 连通 |
| 推理超时 | 模型太大显存不足 | 换量化版 `qwen2.5:7b-q4_K_M` |
| 知识库检索空 | 嵌入模型未装 | `ollama pull bge-m3` 并设为 Embedding 模型 |

## 4. 容器管理

```bash
docker compose logs -f api      # 看日志
docker compose restart worker   # 改完配置重启
```

## 小结

Dify 私有化不复杂，关键是**网络地址**（`host.docker.internal`）和**模型量化**两条。
我在某地项目中沉淀了这套排查 SOP，支撑慢特病预审、AI 导诊等多场景复用。
