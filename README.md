# Enron Governance RAG Pipeline

This project implements a defensible RAG (Retrieval-Augmented Generation) pipeline designed to identify documents within the R. Buy email subset of the Enron email dataset relevant to failures in internal governance. It replicates a legal eDiscovery-style workflow with precision/recall evaluation, chunked semantic retrieval, and optional GPT-3 turbo reranking.

## Purpose

Legal document review systems must balance thoroughness with speed. This notebook demonstrates how to:

- Measure recall using ground truth

- Track false negatives

- Simulate defensible search in investigative and litigation contexts

## Features

- Chunk Enron email content into overlapping segments

- Embed all chunks using BAAI/bge-base-en-v1.5

- Retrieve top-k using FAISS semantic search

- Rerank with BAAI/bge-reranker-large and/or GPT-4

- Evaluate recall, precision, and F1 against labeled ground truth

## Dataset and Ground Truth

### Enron Dataset Source

You can download the full Enron email corpus from sources like:

[Carnegie Mellon University](https://www.cs.cmu.edu/~enron/enron_mail_20150507.tar.gz)

[Kaggle - Enron Email Dataset](https://www.kaggle.com/datasets/wcukierski/enron-email-dataset)

### Preparing the buy-r.zip Subset

This project uses a focused subset of the Enron dataset corresponding to the buy-r mailbox. To create the buy-r.zip used in this notebook:

1. Download and extract the full Enron dataset.

2. Locate the folder named maildir/buy-r.

3. Zip that entire buy-r folder and upload it as buy-r.zip.

The notebook will extract and chunk the contents for embedding.

A hand-curated ground-truth.json file is included with document IDs and a binary "relevant" field. This enables reproducible recall testing across different retrieval strategies. 

**Ground Truth Disclaimer**
The ground-truth.json file provided in this project is not claimed to be authoritative or exhaustive. It was manually curated to enable comparative recall testing and should not be relied on for any production or legal conclusions. Users are encouraged to treat it as a working benchmark, not a gold standard.

Example format:
```
[
  {"doc_id": "buy-r/inbox/431", "relevant": true},
  {"doc_id": "buy-r/inbox/388", "relevant": false},
  ...
]
```

### Requirements
```bash
pip install sentence-transformers faiss-cpu transformers openai
```
### Example Output
```
--- RERANKED Evaluation ---
Recall@20: 0.11
Precision@20: 0.21
F1 Score: 0.15
```
### Future Improvements

- Add keyword-assisted filtering before reranking

- Support TAR 2.0 feedback loop

- Visualize missed document clusters

### Models Used

- BAAI/bge-base-en-v1.5

- BAAI/bge-reranker-large

- Optional: gpt-4 via OpenAI API (if quota is available)

### Legal Tech Notes

This notebook is designed to show the structure, visibility, and rigor required to test whether a recall-based AI pipeline could be defensible in litigation or compliance settings.

Built by an attorney using open-source models to simulate real-world search defensibility.
