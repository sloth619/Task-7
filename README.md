## Track 1 Retrieval Experiments

Given a passage, rank the ELSST concepts in concept_pool.jsonl by relevance. The main metric is **Recall@5**.

## Embedding Zero-shot Results

| Model | Dimension | Recall@5 |
|---|---:|---:|
| voyage-4-large | 256 | 0.2729 |
| voyage-4-large | 512 | 0.3049 |
| voyage-4-large | 1024 | 0.3325 |
| voyage-4-large | 2048 | 0.3567 |
| voyage-4 | 256 | 0.2617 |
| voyage-4 | 512 | 0.3053 |
| voyage-4 | 1024 | 0.3241 |
| voyage-4 | 2048 | 0.3318 |
| gemini-embedding-001 | 3072 | 0.3688 |
| Octen-Embedding-4B | 2560 | 0.3781 |
| Octen-Embedding-8B | 4096 | 0.4059 |
| Qwen3-Embedding-8B | 4096 | 0.3067 |
| Qwen3-Embedding-0.6B | 1024 | 0.2503 |
| BGE large v1.5 | 1024 | 0.2469 |
| llama-embed-nemotron-8b | 4096 | 0.3570 |

## Embedding Fine-tuning Results

| Model | Hyperparameter | Loss | Method | Recall@5 | Recall@10 | Recall@20 |
|---|---|---|---|---|---|---|
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=64 | MNRL | 笛卡尔积 triplets | 0.5295 | 0.6401 | - |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 笛卡尔积 triplets | 0.6015 | 0.7077 | 0.795 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 3 neg/pos triplets | 0.5494 | 0.6552 | 0.7478 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | CachedMNRL | query-positive | 0.5554 | 0.664 | 0.7472 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive InfoNCE | grouped pairs + group_id | 0.5165 | 0.6055 | 0.6902 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Listwise BCE | candidates (pos+neg, binary) | 0.4504 | 0.5514 | 0.6498 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive Softmax | grouped pairs + group_id | 0.5238 | 0.6185 | 0.7208 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | TripletLoss (Margin) | 3 neg/pos triplets | 0.3522 | 0.4367 | 0.509 |
| Octen-Embedding-8B | lr=5e-5, r=16, alpha=32,bs=128 | MNRL | 笛卡尔积 triplets | 0.7398 | 0.8427 | 0.915 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 笛卡尔积 triplets | 0.7564 | 0.8563 | 0.9183 |
| Octen-Embedding-8B | lr=5e-5, r=128, alpha=256,bs=128 | MNRL | 笛卡尔积 triplets | 0.7504 | 0.8574 | 0.9157 |
| Octen-Embedding-8B | lr=5e-5, r=256, alpha=512,bs=128 | MNRL | 笛卡尔积 triplets | 0.7367 | 0.8514 | 0.9162 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 用best ckpt挖掘的hard negatives继续训练 | 0.7699 | 0.8655 | 0.9236 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 用best ckpt挖掘的hard negatives从零训练 | 0.7529 | 0.8572 | 0.9194 |

## Reranker Zero-shot Results (On Octen-Embedding-8B top-20 outputs)
| Model | Recall@5 |
|---|---:|
| Yuan-embedding-2.0-en |0.4556|
| Qwen3-Reranker-0.6B |0.5741|
| Qwen3-Reranker-4B |0.6985|
| Qwen3-Reranker-8B |0.7054|
| voyage rerank-2.5 |0.6405|
| bge-reranker-v2-m3 |0.243|
| Jina Reranker v2 Base Multilingual |0.499|
| bge-reranker-v2-minicpm-layerwise |0.5638|

## Reranker Fine-tuning Results
| Model | Hyperparameter | Loss | Method | Recall@5 | Recall@10 | Recall@20 |
|---|---|---|---|---|---|---|
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-20 候选构造 query-concept pairs，BCE 二分类精排 | 0.8289 | 0.9035 | 0.923 |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-50 候选构造 query-concept pairs，BCE 二分类精排 | 0.84 | 0.9193 | 0.958 |

## Model Ensemble Results
| Model | Hyperparameter | Objective | Method | Recall@5 | Recall@10 | Recall@20 |
|---|---|---|---|---|---|---|
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | k=4, weights=[0.4, 0.6] | RRF | weight: 0.00~1.00 步长 0.05 k: 1~20 每个整数 + 25~100 步长5 + 150/200/300 | 0.8537 | 0.9278 | 0.9592 |

## Track 2 Generate Experiments

Given a passage-level prompt, output the hidden concepts as a short semantic set.

| Model | Hyperparameter | Objective | Method | P | R | F1 |
|---|---|---|---|---|---|---|
| Qwen2.5-7B-Instruct | lr=1e-5,epoch=2, bs=16,tau=0.85 | SFT | Track2 generation baseline, semantic-set generation | 0.6899 | 0.5986 | 0.5827 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | DPO | SFT adapter initialized DPO LoRA, semantic-set generation | 0.8054 | 0.48 | 0.5572 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | ORPO | ORPO LoRA from base model, semantic-set generation | 0.2409 | 0.4348 | 0.2905 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | ORPO | SFT adapter initialized ORPO LoRA, semantic-set generation | 0.8131 | 0.3849 | 0.4834 |



