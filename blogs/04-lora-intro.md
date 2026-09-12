# LoRA 微调入门：让小模型懂医疗问答

> 很多人以为"训练模型"是算法岗的专利。其实 **LoRA 微调**在消费级显卡上就能上手，
> 而且它是你从「调 API」跨向「AI 工程师」最有力的一枚砝码。

## 1. 为什么是 LoRA

全量微调 7B 模型需要多张 A100；LoRA（Low-Rank Adaptation）只训练注入的**低秩适配器**，
基座 70 亿参数冻住不动：

```
原权重 W (d×d)  ──冻住──▶  只训练 A(d×r)·B(r×d)，r 取 8/16
```

好处：显存占用低、训练快、可热插拔、可随时回退——非常适合垂直领域（医疗/政务）小样本适配。

## 2. 最小可跑代码

```python
from peft import LoraConfig, get_peft_model, TaskType
lora = LoraConfig(task_type=TaskType.CAUSAL_LM, r=8, lora_alpha=16,
                  lora_dropout=0.05, target_modules=["q_proj","v_proj"])
model = get_peft_model(model, lora)
model.print_trainable_parameters()   # 通常只训练 <1% 参数
```

## 3. 量化"有没有用"才是关键

微调不是目的，**能证明微调带来提升**才是 AI 工程师素养。用测试集对比基座 vs 微调：

```bash
python src/eval_compare.py
```

我在**本机 CPU（20 核，无 GPU）**上实跑的真实结果：

| 指标 | Base（未微调） | LoRA 微调后 | 提升 |
|---|---|---|---|
| 字符级 bigram F1（主指标） | 0.065 | **0.150** | **+130.3%** |
| 字符级 unigram F1（参考） | 0.162 | 0.327 | +101.9% |

配置：Qwen2.5-0.5B-Instruct + LoRA(r=16, α=32) + 26 条样本 / 10 epoch，
训练 loss 从 2.898 收敛到 0.470，CPU 上 3.5 分钟训完。

**最直观的变化是"口吻"**。同一道题：

> 问：患者仅单次收缩压 145mmHg 能否认定高血压？
>
> - **Base**："是的，单次收缩压达到或超过 140 mmHg 可以被认为是高血压……" ❌ 结论错误
> - **LoRA**："不能。单次收缩压 145/90mmHg 不符合门诊慢特病药品目录认定标准，需到住院证种复核后方可认定。" ✅

微调后模型不只"更像行家"，**结论方向都变对了**。这就是微调的价值。

## 3.5 三个真实的坑（都踩过）

1. **Instruct 模型必须用 chat 模板**。Qwen2.5-Instruct 裸喂 prompt 会乱答——
   实测裸 prompt 下 Base 的 bigram-F1 只有 0.038，套上官方模板后正常。
   更关键的是：**训练和推理要用同一套模板**，否则 train/eval 分布不一致，白训。
2. **CPU 上别用 bfloat16**。Qwen2.5 默认 `torch_dtype=bfloat16`，
   CPU 没有 bf16 硬件指令、走软件模拟，实测单步 **73 秒**；
   改回 `float32` 后单步 **0.5 秒**，快约 150 倍。
3. **推理要传 `eos_token_id`**。否则模型一直续写，后半段全是废话，F1 被稀释。

## 4. 无 GPU 怎么办

**CPU 就能跑通并出数字**（上面那组就是 CPU 跑的）：26 条样本、10 epoch 约 3.5 分钟。
当然想上量还是用 Colab / Kaggle 免费 T4、阿里云 PAI / 魔搭。

> 诚实说明：26 条是极小样本演示，F1 绝对值 0.15 仍偏低，说明模型学会了领域话术与结论倾向，
> 但远未达到可上线水平。真实项目建议 200–1000 条垂直语料 + 7B 基座。

完整代码见仓库 [`medical-lora-finetune`](https://github.com/symQWEQW/medical-lora-finetune)。
