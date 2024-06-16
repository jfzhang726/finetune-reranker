# Finetune-reranker
This code finetunes BGE reranker using Google Colab GPU.

- Base reranker model
  - BAAI/bge-reranker-base
    - Cross-encoder derived from 
  - https://huggingface.co/BAAI/bge-reranker-base
- Finetune dataset
  - Scidoc-reranking dataset, evaluation split
  - https://huggingface.co/datasets/mteb/scidocs-reranking
  - 3.98k rows

- Evaluation dataset
  - Scidoc-reranking dataset, test split
  - 3.98k rows

## Evaluation Results
### Notes:
- **Retrieval**: Baseline retrieval results
  - embedding model: BAAI/bge-base-en-v1.5
  - retrieval method: FAISS, flat, inner product
- **Retrieval + BGE Reranking**: Results from the BGE reranking model BAAI/bge-reranker-base.
- **Retrieval + Finetuned Reranking**: Results from the finetuned reranking model finetuned from BGE reranking model BAAI/bge-reranker-base without being merged to original model.
- **Retrieval + Merged Reranking**: Results from the merged reranking model i.e. the finetuned reranking model is merged to the original reranking model to maintain generalizability.

| Metric              | Retrieval | Retrieval + BGE Reranking | Retrieval + Finetuned Reranking | Retrieval + Merged Reranking |
|---------------------|-----------|---------------|---------------------|------------------|
| **MRR@1**           | 0.1546    | 0.0837        | 0.1335              | 0.1174           |
| **MRR@10**          | 0.2282    | 0.1381        | 0.1974              | 0.1836           |
| **MRR@100**         | 0.2402    | 0.1542        | 0.2112              | 0.1976           |
| **Recall@1**        | 0.0318    | 0.0172        | 0.0274              | 0.0242           |
| **Recall@10**       | 0.1379    | 0.0849        | 0.1145              | 0.1128           |
| **Recall@100**      | 0.3559    | 0.3559        | 0.3559              | 0.3559           |

## Discussion
- For the specific dataset, finetuned reranking model outperforms BGE reranking model
- Merged model (finetuned model + original BGE reranking model) is not as good as finetuned model, still outperforms original model. It is expected: finetuning model on the dataset improves model's performace on the same dataset at the cost of generalizability, and merged model regains some generalizability at the cost of certain performance on the specific dataset.
- It is interesting that retrieval + reranking results are not as good as retrieval only.     
## TODO:
- Retrieval without reranking performs better than retrieval + reranking. Need to investigate.
- Evaluation using RAGAS 
