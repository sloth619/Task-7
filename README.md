# Track 1 Retrieval Experiments

Given a passage, rank the ELSST concepts in concept_pool.jsonl by relevance. The main metric is **NDCG@10**.

## Test Scores
| Model | Hyperparameter | Objective | Method | Val Score | Test Score |
| :--- | :--- | :--- | :--- | :---: | :---: |
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | k=4, weights=[0.4, 0.6] | RRF | weight: 0.00\~1.00 步长 0.05 k: 1\~20 每个整数 + 25\~100 步长5 + 150/200/300 | 0.887 | 0.9219 |
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | k=3, weights=[0.25, 0.75] | RRF | weight: 0.00\~1.00 步长 0.05 k: 1\~20 每个整数 + 25\~100 步长5 + 150/200/300 | 0.8904 | 0.9211 |
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | alpha=0.275, norm=minmax | Weighted Score Fusion | alpha 0.00\~1.00 步长0.02 × norm {minmax, zscore} = 102组合， alpha=retrieval权重， reranker权重=1-alpha | 0.894 | 0.9284 |
| Octen-Embedding-8B | lr=1e-5, r=64, alpha=128,bs=128 | MNRL | 用best ckpt挖掘的hard negatives继续训练 | 0.8176 | 0.862 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 笛卡尔积 triplets | 0.5871 | 0.725 |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-50 候选构造 query-concept pairs, BCE 二分类精排 | 0.8805 | 0.9144 |

## Embedding Zero-shot Results

| Model | Dimension | Recall@5 | NDCG@10 |
| :--- | :---: | :---: | :---: |
| voyage-4-large | 256 | 0.2729 | 0.2687 |
| | 512 | 0.3049 | 0.3141 |
| | 1024 | 0.3325 | 0.3416 |
| | 2048 | 0.3567 | 0.3504 |
| voyage-4 | 256 | 0.2617 | 0.2691 |
| | 512 | 0.3053 | 0.3158 |
| | 1024 | 0.3241 | 0.3455 |
| | 2048 | 0.3318 | 0.3708 |
| gemini-embedding-001 | 3072 | 0.3688 | 0.3735 |
| Octen-Embedding-4B | 2560 | 0.3781 | 0.3889 |
| Octen-Embedding-8B | 4096 | 0.4059 | 0.4204 |
| Qwen3-Embedding-8B | 4096 | 0.3067 | 0.3057 |
| Qwen3-Embedding-0.6B | 1024 | 0.2503 | 0.2581 |
| BGE large v1.5 | 1024 | 0.2469 | 0.2578 |
| llama-embed-nemotron-8b | 4096 | 0.357 | 0.3700 |

## Embedding Fine-tuning Results

| Model | Hyperparameter | Loss | Method | Recall@5 | Recall@10 | Recall@20 | NDCG@10 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=64 | MNRL | 笛卡尔积 triplets | 0.5295 | 0.6401 | - | - |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 笛卡尔积 triplets | 0.6015 | 0.7077 | 0.795 | 0.5871 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | MNRL | 3 neg/pos triplets | 0.5494 | 0.6552 | 0.7478 | 0.5858 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | CachedMNRL | query-positive | 0.5554 | 0.664 | 0.7472 | 0.5911 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive InfoNCE | grouped pairs + group_id | 0.5165 | 0.6055 | 0.6902 | 0.5507 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Listwise BCE | candidates (pos+neg, binary) | 0.4504 | 0.5514 | 0.6498 | 0.467 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | Multi-positive Softmax | grouped pairs + group_id | 0.5238 | 0.6185 | 0.7208 | 0.5533 |
| Qwen3-Embedding-0.6B | lr=1e-4 r=16, alpha=32,bs=384 | TripletLoss (Margin) | 3 neg/pos triplets | 0.3522 | 0.4367 | 0.509 | 0.3708 |
| Octen-Embedding-8B | lr=5e-5, r=16, alpha=32,bs=128 | MNRL | 笛卡尔积 triplets | 0.7398 | 0.8427 | 0.915 | 0.7921 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 笛卡尔积 triplets | 0.7564 | 0.8563 | 0.9183 | 0.8085 |
| Octen-Embedding-8B | lr=5e-5, r=128, alpha=256,bs=128 | MNRL | 笛卡尔积 triplets | 0.7504 | 0.8574 | 0.9157 | 0.7884 |
| Octen-Embedding-8B | lr=5e-5, r=256, alpha=512,bs=128 | MNRL | 笛卡尔积 triplets | 0.7367 | 0.8514 | 0.9162 | 0.7832 |
| Octen-Embedding-8B | lr=1e-5, r=64, alpha=128,bs=128 | MNRL | 用best ckpt挖掘的hard negatives继续训练 | 0.7699 | 0.8655 | 0.9236 | 0.8176 |
| Octen-Embedding-8B | lr=5e-5, r=64, alpha=128,bs=128 | MNRL | 用best ckpt挖掘的hard negatives从零训练 | 0.7529 | 0.8572 | 0.9194 | 0.8032 |
| Qwen3-Embedding-8B | lr=5e-5, r=64, alpha=128, bs=128 | MNRL | 笛卡尔积 triplets  | 0.7431  | 0.8507  | 0.9153 | 0.7970 |

## Reranker Zero-shot Results (On Octen-Embedding-8B top-20 outputs)
| Model | Recall@5 | NDCG@10 |
| :--- | :---: | :---: |
| Yuan-embedding-2.0-en | 0.4556 | 0.4826 |
| Qwen3-Reranker-0.6B | 0.5741 | 0.602 |
| Qwen3-Reranker-4B | 0.6985 | 0.7444 |
| Qwen3-Reranker-8B | 0.7054 | 0.7439 |
| voyage rerank-2.5 | 0.6405 | 0.6713 |
| bge-reranker-v2-m3 | 0.243 | 0.303 |
| Jina Reranker v2 Base Multilingual | 0.49902 | 0.5443 |
| bge-reranker-v2-minicpm-layerwise | 0.5638 | 0.6099 |

## Reranker Fine-tuning Results
| Model | Hyperparameter | Loss | Method | Recall@5 | Recall@10 | Recall@20 | NDCG@10 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-20 候选构造 query-concept pairs，BCE 二分类精排 | 0.8289 | 0.9035 | 0.923 | 0.8675 |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-50 候选构造 query-concept pairs，BCE 二分类精排 | 0.84 | 0.9193 | 0.958 | 0.8805 |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128,bs=128 | BCE | Octen Top-20 包含所有正例 | 0.8297 | 0.9032 | 0.923 | 0.8674 |
| Qwen3-Reranker-8B | lr=2e-5, r=64, alpha=128, bs=128, | Pairwise Margin | Octen Top-50 候选构造 query-concept pairs，Pairwise Margin 精排 | 0.7958 | 0.8979 | 0.9503 | 0.8442 |

## Model Ensemble Results
| Model | Hyperparameter | Objective | Method | Recall@5 | Recall@10 | Recall@20 | NDCG@10 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | k=4, weights=[0.4, 0.6] | RRF | weight: 0.00\~1.00 步长 0.05 k: 1\~20 每个整数 + 25\~100 步长5 + 150/200/300 | 0.8537 | 0.9278 | 0.9592 | 0.887 |
| Octen-Embedding-8B (0.77) + Qwen3-Reranker-8B (0.84) | alpha=0.16, norm=zscore | Weighted Score Fusion | alpha 0.00\~1.00 步长0.02 × norm {minmax, zscore} = 102组合，alpha=retrieval权重，reranker权重=1-alpha | 0.8576 | 0.9291 | 0.9596 | 0.8931 |

# Track 2 Generate Experiments

Given a passage-level prompt, output the hidden concepts as a short semantic set.The main metric is **F1**.

## Test Scores
| Model | Hyperparameter | Objective | Method | Val Score | Test Score |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Qwen2.5-7B-Instruct | lr=1e-5,epoch=2, bs=16,tau=0.85 | SFT | Track2 generation baseline, semantic-set generation | 0.5827 | 0.6082 |
| ScoreFusion | K=3, no LLM | - | Track1 retriever top-K as Track2 prediction | 0.7359 | 0.7602 |

## Fine-tuning Results
| Model | Hyperparameter | Objective | avg_pred | Method | P | R | F1 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Qwen2.5-7B-Instruct | lr=1e-5, epoch=2, bs=16, tau=0.85 | SFT | - | Track2 generation baseline, semantic-set generation | 0.6899 | 0.5986 | 0.5827 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | DPO | - | SFT adapter initialized DPO LoRA, semantic-set generation | 0.8054 | 0.48 | 0.5572 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | ORPO | - | ORPO LoRA from base model, semantic-set generation | 0.2409 | 0.4348 | 0.2905 |
| Qwen2.5-7B-Instruct | lr=5e-6, epoch=1, bs=16, beta=0.1, tau=0.85 | ORPO | - | SFT adapter initialized ORPO LoRA, semantic-set generation | 0.8131 | 0.3849 | 0.4834 |
| Qwen2.5-7B-Instruct | lr=1e-5, epoch=2, bs=16, r=16, alpha=32, top-K=5 | Candidate SFT | 2.43 | Track1 ScoreFusion top-5 + SFT selection | 0.8968 | 0.7639 | 0.7805 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=16, alpha=32, top-K=5 | Candidate SFT | 2.83 | Track1 ScoreFusion top-5 + SFT selection | 0.8658 | 0.8571 | 0.8287 |
| Qwen3.5-9B | lr=1e-5, epoch=2, bs=32, r=16, alpha=32, top-K=5 | Candidate SFT | 2.63 | Track1 ScoreFusion top-5 + SFT selection | 0.8659 | 0.8104 | 0.7978 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=16, alpha=32, top-K=5 | Candidate SFT | 2.52 | Track1 ScoreFusion top-5 + SFT selection | 0.9219 | 0.8207 | 0.8400 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=16, alpha=32, top-K=7 | Candidate SFT | 2.74 | Track1 ScoreFusion top-7 + SFT selection | 0.8968 | 0.8412 | 0.8412 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=16, alpha=32, top-K=10 | Candidate SFT | 2.8 | Track1 ScoreFusion top-10 + SFT selection | 0.9069 | 0.8697 | 0.8654 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=64, alpha=128, top-K=10 | Candidate SFT | 2.77 | Track1 ScoreFusion top-10 + SFT selection | 0.9276 | 0.8797 | 0.8850 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=128, alpha=256, top-K=10 | Candidate SFT | 2.72 | Track1 ScoreFusion top-10 + SFT selection | 0.9403 | 0.8806 | 0.8945 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=256, alpha=512, top-K=10 | Candidate SFT | 2.62 | Track1 ScoreFusion top-10 + SFT selection | 0.9370 | 0.8807 | 0.8987 |
| Qwen3.5-4B | lr=1e-5, epoch=2, bs=16, r=512, alpha=1024, top-K=10 | Candidate SFT | 2.76 | Track1 ScoreFusion top-10 + SFT selection | 0.9389 | 0.8915 | 0.9016 |

## Track1 Retriever Top-K as Track2 Prediction (Zero-Shot LLM)
| Model | Top-k | avg_pred | P | R | F1 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Qwen2.5-7B-Instruct | 20 | 2.99 | 0.6338 | 0.6591 | 0.5739 |
| Qwen3-4B-Instruct-2507 | 20 | 2.41 | 0.2333 | 0.4054 | 0.2707 |
| Qwen3.5-4B | 20 | 4.97 | 0.5095 | 0.8848 | 0.6099 |
| Qwen3-8B | 20 | 5 | 0.5215 | 0.9068 | 0.6248 |
| Qwen3.5-9B | 20 | 4.98 | 0.5267 | 0.9045 | 0.6276 |
| Qwen2.5-7B-Instruct | 5 | 2.82 | 0.7696 | 0.7918 | 0.728 |
| Qwen3.5-9B | 5 | 4.17 | 0.6315 | 0.9096 | 0.6983 |
| Qwen3.5-4B | 5 | 4.35 | 0.6148 | 0.9211 | 0.6912 |
| Qwen3-8B | 5 | 4.47 | 0.6023 | 0.9221 | 0.6815 |

## Track1 retriever top-K as Track2 prediction(No LLM)

| Model | Hyperparameter | avg_pred | Method | P | R | F1 |
| :--- | :--- | :---: | :--- | :---: | :---: | :---: |
| ScoreFusion | K=1 | 1 | Track1 retriever top-K as Track2 prediction | 0.9894 | 0.4549 | 0.5769 |
| ScoreFusion | K=2 | 2 | Track1 retriever top-K as Track2 prediction | 0.8638 | 0.6909 | 0.714 |
| ScoreFusion | K=3 | 3 | Track1 retriever top-K as Track2 prediction | 0.7478 | 0.8295 | 0.7359 |
| ScoreFusion | K=4 | 4 | Track1 retriever top-K as Track2 prediction | 0.6419 | 0.9073 | 0.707 |
| ScoreFusion | K=5 | 5 | Track1 retriever top-K as Track2 prediction | 0.5516 | 0.9506 | 0.6593 |
| ScoreFusion | K=5, threshold=0.75 | 2.88 | Track1 ScoreFusion top-5, score threshold filtering | 0.8318 | 0.8209 | 0.7837 |

## Count Regressor (On ScoreFusion)
| 模型 | n accuracy | MAE | avg_pred | Sem P | Sem R | Sem F1 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Ridge | 29.6% | 0.94 | 2.97 | 0.7883 | 0.8426 | 0.7777 |
| RF | 31.2% | 0.95 | 2.99 | 0.7950 | 0.8439 | 0.7807 |
| GBR | 28.7% | 1.04 | 2.98 | 0.7923 | 0.8342 | 0.7699 |
| GBC | 34.4% | 1.11 | 2.99 | 0.8108 | 0.8140 | 0.7607 |

