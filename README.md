# Daily Planet AI Desk — Production RAG Capstone

> An end-to-end, production-grade Retrieval-Augmented Generation (RAG) system built for the Daily Planet newsroom archive.

---

## 📰 Project Overview

The **Daily Planet AI Desk** is an AI assistant engineered to query, analyze, and synthesize knowledge from a continuously evolving newsroom archive. Built across a structured 6-week engineering roadmap, this capstone moves beyond naive toy implementations to deliver an enterprise-grade RAG pipeline featuring:

- **Intelligent Ingestion & Chunking** for messy multi-format real-world news documents.
- **Production Vector Storage** migrating from local FAISS to self-hosted Qdrant in Docker.
- **Hybrid Retrieval & Cross-Encoder Reranking** uniting semantic vectors with BM25 exact keyword matching.
- **Grounded Generation & Inline Citations** enforcing strict context attribution and honest refusal fallback patterns (`"I cannot verify that"`).
- **Security & Data Freshness** incorporating metadata-based per-user access control and continuous incremental re-indexing.
- **Rigorous Evaluation & Observability** powered by golden test sets, Hit-Rate / MRR metrics, LLM-as-a-judge faithfulness verification, and Langfuse tracing.

---

## 🏛️ Pipeline Architecture

The system follows a strict 6-stage production flow:

```
[ Raw Documents ] (PDF, HTML, MD, Wire)
       │
       ▼
 1. INGESTION        ── Loaders strip boilerplate & extract metadata (source, date, author)
       │
       ▼
 2. CHUNKING         ── Recursive & semantic token-aware splitting preserving document lineage
       │
       ▼
 3. EMBED & STORE    ── OpenAI Embeddings stored in Qdrant Vector DB & BM25 inverted index
       │
       ▼
 4. HYBRID RETRIEVAL ── Dense vector search + BM25 keyword search fused & reranked (Cross-Encoder / Cohere)
       │
       ▼
 5. GROUNDED GENERATION ─ Strict prompt attribution with inline citations & honest fallback refusal
       │
       ▼
 6. EVAL & OBSERVABILITY ─ Langfuse request tracing, hit-rate/MRR scoring, & LLM-as-judge faithfulness
```

---

## 🗓️ 6-Week Roadmap & Implementation Flow

### **Week 1 · Why RAG, Ingestion & Chunking**
- **Objective:** Establish the data foundation that determines whether downstream retrieval works.
- **What You Build:** Core ingestion module that loads the newsroom corpus (PDFs, HTML wire feeds, Markdown) and partitions text into well-formed, metadata-tagged chunks.
- **Topics Covered:**
  - Why RAG beats fine-tuning and long-context windows for grounded, source-attributable, and constantly updated news knowledge.
  - The 6-stage RAG pipeline (`Ingest -> Chunk -> Embed -> Store -> Retrieve -> Generate`).
  - Document loaders handling real-world messiness (boilerplate, HTML tags, headers, frontmatter).
  - Chunking strategies: fixed-length, recursive character, and semantic splitting.
  - Chunk size and overlap tuning to avoid context truncation and retrieval pollution.
- **Milestone Deliverable:** `rag/ingest.py` and `rag/chunk.py`.
- **Assessment:** 7-question practice quiz, discussion forum, and live session.

---

### **Week 2 · Embeddings & Vector Search**
- **Objective:** Enable semantic meaning-based retrieval across newsroom archives.
- **What You Build:** Semantic search engine over chunked articles, starting locally with FAISS before migrating to Qdrant running in Docker.
- **Topics Covered:**
  - Text embeddings, vector spaces, and cosine similarity mechanics.
  - OpenAI embedding model selection (`text-embedding-3-small` vs. `text-embedding-3-large`) balancing cost, latency, and dimensional trade-offs.
  - Local vector prototyping with FAISS.
  - Migration to dedicated Qdrant vector database in Docker for production persistence and filtering.
  - Explainability: inspecting query-to-chunk cosine similarity scores to understand *why* a chunk was retrieved.
- **Milestone Deliverable:** Vector indexing scripts and Qdrant connector (`rag/vector_store.py`).
- **Assessment:** 7-question practice quiz, discussion forum, and live session.

---

### **Week 3 · Retrieval Strategies: Hybrid Search & Reranking**
- **Objective:** Make retrieval robust against exact-term misses and ensure the most relevant chunks rise to the top.
- **What You Build:** A hybrid retriever combining sparse keyword search (BM25) and dense vector search, followed by a cross-encoder / Cohere reranker.
- **Topics Covered:**
  - The exact-match limitation of pure vector search (journalistic names, acronyms, ticker symbols, dates).
  - BM25 inverted index implementation.
  - Reciprocal Rank Fusion (RRF) / score fusion combining dense and sparse retriever outputs.
  - Two-stage retrieval pattern: *Retrieve broad (high Top-K) $\rightarrow$ Rerank precise (Top-N)*.
  - Qualitative evaluation of rank improvements.
- **Milestone Deliverable:** Hybrid retriever and reranking pipeline (`rag/retriever.py`).
- **Assessment:** 7-question practice quiz, discussion forum, and live session.

---

### **Week 4 · Grounded Generation: Citations & Honest Fallbacks**
- **Objective:** Turn retrieved chunks into faithful, cited responses while eliminating hallucinations.
- **What You Build:** A unified answering endpoint that synthesizes evidence into inline citations or triggers an honest refusal when evidence is absent.
- **Topics Covered:**
  - Assembling retrieved chunks into structured prompts that restrict the LLM to provided context.
  - Enforcing claim attribution with inline source markers (e.g., `[source.md::chunk_0]`).
  - Low-confidence retrieval detection: implementing explicit fallbacks (*"I cannot verify that from the available archive"*).
  - Why correct refusal is critical in high-stakes newsroom reporting over plausible guessing.
  - Exposing the unified flow (`Retrieve -> Rerank -> Generate`) behind a single endpoint.
- **Milestone Deliverable:** Grounded generation module and RAG execution service (`rag/generate.py`, `rag/pipeline.py`).
- **Assessment:** 7-question practice quiz, discussion forum, and live session.

---

### **Week 5 · Access Control, Data Freshness & The Managed Path**
- **Objective:** Harden security, manage corpus lifecycle, and evaluate cloud architectural trade-offs.
- **What You Build:** Per-user role filtering in the vector store and an incremental re-indexing pipeline for new and edited stories.
- **Topics Covered:**
  - Enforcing access control at the retrieval layer via vector metadata filters (preventing unauthorized document leaks).
  - Privacy risks of relying on prompt-level access control versus storage-level filtering.
  - Incremental re-indexing and data freshness strategies for continuously publishing newsrooms.
  - Conceptual architecture of GraphRAG for interconnected entity and relationship queries.
  - Build vs. Buy evaluation: comparing self-built RAG (Qdrant + self-hosted pipeline) against managed offerings (Pinecone, AWS Bedrock Knowledge Bases, AWS OpenSearch Service).
- **Milestone Deliverable:** Role-filtered search and re-indexing pipeline (`rag/access.py`, `rag/reindex.py`).
- **Assessment:** 7-question practice quiz, discussion forum, and live session.

---

### **Week 6 · Retrieval Evaluation & The Production RAG Capstone**
- **Objective:** Scientifically measure retrieval and generation accuracy, establish tracing, and deliver the final capstone.
- **What You Build:** An automated evaluation harness with regression checks and local Langfuse observability tracing.
- **Topics Covered:**
  - Creating a golden evaluation dataset (newsroom test questions paired with ground-truth chunks and target answers).
  - Measuring retrieval performance: **Hit-Rate** and **Mean Reciprocal Rank (MRR)**.
  - Measuring answer quality: **Faithfulness** and **Context Relevance** via LLM-as-a-judge.
  - Observability with Langfuse (local instance): tracing latency, token consumption, and per-query costs.
  - Automated regression testing: detecting when prompt, chunking, or model adjustments degrade system performance.
- **Milestone Deliverable:** Evaluation suite and Capstone project demonstration (`rag/evaluate.py`, Langfuse dashboard integration).
- **Assessment:** 7-question practice quiz, discussion forum, live session, and final Module 2 capstone presentation.

---

## 📁 Repository Structure

```text
daily_planet_ai_desk/
├── 6_week_plan.md        # Technical syllabus & weekly learning objectives
├── Handbook.md           # Learner guide with build milestones & assessments
├── README.md             # Project documentation and capstone scope
├── data/                 # Raw newsroom archives
│   ├── articles/         # Markdown & text stories with frontmatter metadata
│   └── wire/             # HTML syndicated news wire feeds
└── rag/                  # Core RAG implementation modules
    ├── __init__.py
    ├── ingest.py         # [Week 1] Multi-format document loading & metadata parsing
    ├── chunk.py          # [Week 1] Recursive token-aware text splitting
    ├── vector_store.py   # [Week 2] FAISS and Qdrant vector storage & embeddings
    ├── retriever.py      # [Week 3] BM25 + Dense Hybrid search & Cross-Encoder reranking
    ├── generate.py       # [Week 4] Prompt templates, inline citations & honest refusals
    ├── pipeline.py       # [Week 4] Unified query-to-answer RAG execution service
    ├── access.py         # [Week 5] Role-based metadata filtering & security
    ├── reindex.py        # [Week 5] Incremental indexing & freshness sync
    └── evaluate.py       # [Week 6] Golden set runner, MRR/Hit-Rate & Langfuse tracing
```

---

## 🚀 Setup & Getting Started

### 1. Prerequisites
- Python 3.10+
- Docker & Docker Compose (for Qdrant & Langfuse)
- OpenAI API Key (and optional Cohere API Key for reranking)

### 2. Environment Configuration
Create a `.env` file in the root directory:
```bash
OPENAI_API_KEY=your_openai_api_key
COHERE_API_KEY=your_cohere_api_key_optional
QDRANT_HOST=localhost
QDRANT_PORT=6333
LANGFUSE_PUBLIC_KEY=your_langfuse_public_key
LANGFUSE_SECRET_KEY=your_langfuse_secret_key
LANGFUSE_HOST=http://localhost:3000
```

### 3. Launching Infrastructure
Start Qdrant vector database:
```bash
docker run -d -p 6333:6333 -p 6334:6334 \
    -v $(pwd)/qdrant_storage:/qdrant/storage:z \
    qdrant/qdrant
```

### 4. Running the Pipeline (Weeks 1–6)
- **Ingest & Chunk documents:**
  ```python
  from rag.ingest import ingest_folder
  from rag.chunk import chunk_documents

  docs = ingest_folder("data")
  chunks = chunk_documents(docs, chunk_size=400, chunk_overlap=50)
  print(f"Ingested {len(docs)} documents into {len(chunks)} chunks.")
  ```
- **Run the full test and evaluation suite:**
  ```bash
  python -m rag.evaluate
  ```
