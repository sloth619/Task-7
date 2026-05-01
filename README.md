## Track 1 Retrieval Experiments

This section reports current Track 1 retrieval results on the ELSST validation set.

The task is to retrieve relevant ELSST concepts from a fixed concept pool given a long social-science passage. The main metric is **Recall@5**.

## Zero-shot Results

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

## Fine-tuning Results

| Model | Hyperparameter | Method | Recall@5 | Recall@10| Recall@20
|---|---|---|---|---|---:|
| Qwen3-Embedding-0.6B |lr=1e-4 r=16, alpha=32, dropout=0.05 | query-positive-hard negative triplet fine-tuning | 0.5295 |0.6401 |-
| Octen-Embedding-8B | lr=5e-5,  r=16, alpha=32, dropout=0.05| query-positive-hard negative triplet fine-tuning | 0.7398 |0.8427|0.9150
| Octen-Embedding-8B | lr=5e-5,  r=64, alpha=128, dropout=0.05| query-positive-hard negative triplet fine-tuning | 0.7421 |0.8563|0.9183
