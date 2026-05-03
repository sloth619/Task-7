## Track 1 Retrieval Experiments

This section reports current Track 1 retrieval results on the ELSST validation set.

The task is to retrieve relevant ELSST concepts from a fixed concept pool given a long social-science passage. The main metric is **Recall@5**.

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

| Model | Hyperparameter | Loss | Data | Recall@5 | Recall@10 | Recall@20 |
|---|---|---|---|---|---|---|
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=64 | MNRL | 笛卡尔积 triplets | 0.5295 | 0.6401 | - |
| Octen-Embedding-8B | lr=5e-5, r=16, alpha=32,bs=128 | MNRL | 笛卡尔积 triplets | 0.7398 | 0.8427 | 0.915 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 笛卡尔积 triplets | 0.7564 | 0.8563 | 0.9183 |
| Octen-Embedding-8B | lr=5e-5, r=256, alpha=256,bs=128 | MNRL | 笛卡尔积 triplets | 0.7367 | 0.8514 | 0.9162 |
| Octen-Embedding-8B | lr=5e-5, r=128, alpha=256,bs=128 | MNRL | 笛卡尔积 triplets | 0.7367 | 0.8514 | 0.9162 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 3 neg/pos triplets | 0.5494 | 0.6552 | 0.7478 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | CachedMNRL | query-positive | 0.5554 | 0.664 | 0.7472 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 笛卡尔积 triplets | 0.6015 | 0.7077 | 0.795 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive InfoNCE | grouped pairs + group_id | 0.5165 | 0.6055 | 0.6902 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Listwise BCE | candidates (pos+neg, binary) | 0.4504 | 0.5514 | 0.6498 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive Softmax | grouped pairs + group_id | 0.5238 | 0.6185 | 0.7208 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | TripletLoss (Margin) | 3 neg/pos triplets | 0.3522 | 0.4367 | 0.509 |

## Reranker Zero-shot Results (On Octen-Embedding-8B top-20 outputs)
| Model | Recall@5 |
|---|---:|
| Yuan-embedding-2.0-en |0.4556|
| Qwen3-Reranker-0.6B |0.5741|
| Qwen3-Reranker-4B |0.6985|
| Qwen3-Reranker-8B |0.7054|

