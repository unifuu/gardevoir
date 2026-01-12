---
title: About RAG
date: 2026-01-02
summary: 📝 Quick notes on RAG
tags:
  - ai
  - agent
  - llm
  - rag
---

## 1. Introduction

Retrieval-Augmented Generation (RAG) is an architectural framework that improves the accuracy and reliability of Large Language Models (LLMs) by grounding them in external, authoritative data sources.

While LLMs are powerful reasoning engines, they suffer from two primary limitations:

1. **Knowledge Cutoff:** Their internal knowledge is static and limited to the date their training ended.
2. **Hallucinations:** When facing gaps in knowledge, they may confidently invent incorrect information.

RAG bridges this gap by allowing the model to reference a specific database of information before generating a response, similar to a student taking an open-book exam.

---

## 2. Core Architecture: How RAG Works

The RAG process can be broken down into two main phases: **Indexing** and **Retrieval & Generation**.

### Phase 1: Indexing (Data Preparation)

Before the system can answer questions, the external data must be processed and stored.

1. **Document Loading:** Data is imported from various sources (PDFs, HTML, APIs).
2. **Splitting (Chunking):** Large documents are broken down into smaller pieces called "chunks" to fit the LLM's context window.
3. **Embedding:** An Embedding Model converts these text chunks into numerical vectors that represent semantic meaning.
4. **Storage:** These vectors are stored in a Vector Database (e.g., Pinecone, ChromaDB), allowing for fast similarity searching.

### Phase 2: Retrieval & Generation (The Workflow)

1. **User Query:** The user inputs a prompt.
2. **Query Embedding:** The system converts the user's question into a numerical vector.
3. **Retrieval:** The system searches the Vector Database for text chunks mathematically most similar to the question.
4. **Augmentation:** The system combines the User Query and the Retrieved Context into a single prompt.
5. **Generation:** The LLM processes this augmented prompt and generates a factual answer based on the provided data.

---

## 3. Key Components of a RAG Stack

| Component           | Role                                         | Examples                              |
| :------------------ | :------------------------------------------- | :------------------------------------ |
| **LLM**             | The reasoning engine that generates text.    | GPT-4, Claude 3, Llama 3              |
| **Vector Database** | The memory where searchable data is stored.  | Pinecone, Milvus, Weaviate            |
| **Embedding Model** | The translator that turns text into vectors. | OpenAI text-embedding-3, Cohere Embed |
| **Orchestrator**    | The glue connecting all components.          | LangChain, LlamaIndex                 |

---

## 4. RAG vs. Fine-Tuning

| Feature              | RAG                            | Fine-Tuning                         |
| :------------------- | :----------------------------- | :---------------------------------- |
| **Knowledge Source** | External data (Vector DB).     | Internal model weights.             |
| **Data Freshness**   | Real-time updates.             | Requires re-training.               |
| **Transparency**     | Can provide citations/sources. | Black box (no clear source).        |
| **Best Use Case**    | Fact-based retrieval.          | Learning style or specific formats. |

---

## 5. Benefits of RAG

- **Accuracy:** Significantly reduces hallucinations by grounding answers in facts.
- **Cost-Efficiency:** Cheaper than retraining models for new data.
- **Security:** Allows models to use private company data without incorporating it into the public training set.
- **Verifiability:** Allows the system to cite specific documents used for the answer.

---

## 6. Challenges and Limitations

- **Retrieval Quality:** If the search finds irrelevant data, the answer will be wrong (Garbage In, Garbage Out).
- **Latency:** Adding a retrieval step adds a small amount of processing time.
- **Complexity:** Requires maintaining a vector database and an embedding pipeline.

---

## 7. Advanced RAG Techniques

To improve performance, developers use advanced methods:

- **Hybrid Search:** Combining vector search with traditional keyword search.
- **Re-ranking:** Using a second model to rank the most relevant documents before giving them to the LLM.
- **Query Expansion:** Rewriting a user's question to make it more descriptive for better search results.
