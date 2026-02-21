# Hallucination RAG Assistant

A literature-grounded Retrieval-Augmented Generation (RAG) system specialized in hallucination and faithfulness research in Large Language Models (LLMs).

This project builds a citation-aware research assistant over 120 curated arXiv papers (2020–2026) and enables:

- Grounded question answering with inline citations
- Paper-to-paper comparison

## Project Motivation

Hallucination and faithfulness remain central challenges in modern LLM systems.

While many RAG demos exist, few focus on:

- Literature-grounded answers
- Citation transparency
- Research paper comparison
- Safety-aware context filtering

This system is designed as a research-oriented assistant for analyzing academic papers on:

- Hallucination
- Faithfulness
- Groundedness
- Factual consistency
- Attribution in LLMs

## Dataset

- 120 curated arXiv papers
- Years: 2020–2026
- Categories: `cs.CL`, `cs.AI`, `cs.IR`, `cs.LG`

## System Architecture

1. arXiv PDFs
2. Chunking + Cleaning
3. Embedding (SentenceTransformer) BAAI/bge-base-en-v1.5
4. Chroma Vector DB
5. Context Builder (Injection Filtering)
6. Local LLM (Ollama - qwen2.5:7b-instruct)
7. Citation-Grounded Answer

#### Planned

Generation eval: citation precision, groundedness score, RAGAS

## Core Components

### Retrieval

- Embedding-based similarity search

### Context Construction

- Chunk filtering (remove references, prompt injection patterns)
- Citation headers: `[paper_id p.page]`
- Character-limited context assembly

### Local LLM Integration

Using Ollama:
OLLAMA_URL = "http://localhost:11434/api/generate"
OLLAMA_MODEL = "qwen2.5:7b-instruct"

## Example Output

### Query to ask a question

```
res = answer("How do papers define faithfulness or groundedness?", k=10)
print(res["answer"])
```

Output:

```text
Faithfulness or groundedness is defined as a multi-party property where a faithful explanation enables a listener model to come to the same conclusion as the speaker without access to the speaker’s answer. Specifically, faithfulness is framed in terms of reasoning executability: a reasoning chain is considered faithful if it can be executed by a similarly capable listener to recover the same conclusion, without access to the speaker's final answer.

Sources:

- [2602.16154v1 p.3]
- [2602.16154v1 p.9]
```

### Query to compare

```
cmp_res = compare_papers(
"How is faithfulness defined and measured?",
paper_ids=["2602.16154v1", "2602.14529v1"]
)
print(cmp_res["answer"])
```

Output:

```text
### Summary Table

| Paper ID | Key Points |
| --- | --- |
| 2602.16154v1 p.3 | Faithfulness is defined as the degree to which a listener model reaches the same answer as the original speaker reasoning model. The faithfulness reward is computed by comparing the answers of multiple listeners with the original answer. |
| 2602.16154v1 p.6 | REMUL trains models for both faithfulness and correctness simultaneously, using truncated CoT answering and adding mistakes to measure faithfulness. Faithfulness-only training improves AOC metrics, while hint-optimized models perform worse on these metrics. |
| 2602.16154v1 p.1 | Faithfulness is a multi-party property where the speaker's reasoning must be executable by a listener without access to the answer. REMUL uses soft execution and multiple listeners for training. |

### Agreement / Disagreement

- **Agreement**: Both papers agree that faithfulness involves ensuring the listener model reaches the same conclusion as the speaker, even when only part of the reasoning chain is provided.
- **Disagreement**: The first paper (2602.16154v1 p.3) provides a more detailed definition and computation method for faithfulness, while the second paper (2602.16154v1 p.6) focuses on the metrics used to measure it.

### Practical Takeaway

- **Definition of Faithfulness**: Faithfulness is crucial in ensuring that AI models provide explanations or reasoning chains that are understandable and executable by other models, even when parts of the chain are truncated.
- **Training Methods**: Training methods like REMUL (2602.16154v1 p.3) can balance faithfulness with correctness, while metrics like AOC and hint usage help in measuring these properties effectively.

By focusing on both the definition and practical training methods for faithfulness, researchers can ensure that AI models provide clear and executable reasoning chains, enhancing their overall utility and reliability.
```

## Installation and run MVP

### 1. Clone repository

```
git clone https://github.com/yourname/hallucination-rag-assistant.git
cd hallucination-rag-assistant
```

### 2. Install dependencies

```
pip install -r requirements.txt
```

### 3. Install Ollama

Download from:ollama.com

Pull model:

```
ollama pull qwen2.5:7b-instruct
```

### 4. Install and run notebook

05_build_MVP.ipynb
