# Daily Planet AI Desk — 6-Week Implementation Project Plan

This document outlines the detailed Kanban board structure, current live implementation status, and work breakdown for the 6-Week Production RAG Capstone.

---

## 📊 Live Implementation Status Dashboard

| Milestone / Week | Tasks Progress | Status | Milestone Goal | Deliverables / Files |
| :--- | :---: | :---: | :--- | :--- |
| **Week 1 · Ingestion & Chunking** | **3 / 3** | **✅ Done** | Multi-format loading & token-aware chunking | [`rag/ingest.py`](file:///home/samiran/projects/agentic-ai-learning/daily_planet_ai_desk/rag/ingest.py), [`rag/chunk.py`](file:///home/samiran/projects/agentic-ai-learning/daily_planet_ai_desk/rag/chunk.py), [`data/`](file:///home/samiran/projects/agentic-ai-learning/daily_planet_ai_desk/data) |
| **Week 2 · Embeddings & Vector Search** | **0 / 4** | **⏳ Todo** | Semantic search with FAISS & Qdrant in Docker | `rag/vector_store.py`, Docker configuration |
| **Week 3 · Hybrid Search & Reranking** | **0 / 4** | **⏳ Todo** | BM25 sparse + Dense vector fusion & Cross-Encoder | `rag/retriever.py` |
| **Week 4 · Grounded Generation & Citations** | **0 / 5** | **⏳ Todo** | Context-constrained prompts, citations & refusals | `rag/generate.py`, `rag/pipeline.py` |
| **Week 5 · Access Control & Data Freshness** | **0 / 5** | **⏳ Todo** | RBAC metadata filtering & incremental re-indexing | `rag/access.py`, `rag/reindex.py` |
| **Week 6 · Evaluation & Production Capstone** | **0 / 5** | **⏳ Todo** | Golden dataset, Hit-rate/MRR, Langfuse tracing | `rag/evaluate.py`, Capstone presentation |

---

## Kanban Board Columns

1. **Backlog / Todo** — Planned items ready to be picked up.
2. **In Progress** — Actively being implemented or tested.
3. **Review / Verification** — Code review, quiz completion, or evaluation regression checks.
4. **Done** — Merged, tested, and validated deliverables.

---

## Work Breakdown Structure & Kanban Cards

### **Week 1: Document Ingestion & Intelligent Chunking**

#### Task 1.1: Multi-Format Document Ingestion Loader (`rag/ingest.py`)
- **GitHub Issue:** [#1](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/1)
- **Status:** **✅ Done (Closed)**
- **Type:** Feature / Foundation
- **Description:** Build a unified ingestion pipeline capable of loading raw, messy newsroom files across multiple formats (Markdown with YAML frontmatter, HTML syndicated wire articles, and PDFs).
- **Key Deliverables:**
  - `load_markdown()` parsing frontmatter metadata (title, author, date, section).
  - `load_html()` using BeautifulSoup to strip navigation, ads, headers, and footers.
  - Standardized `Document(text, metadata)` container preserving document provenance.
- **Acceptance Criteria:**
  - [x] Implemented in [`rag/ingest.py`](file:///home/samiran/projects/agentic-ai-learning/daily_planet_ai_desk/rag/ingest.py).
  - [x] Successfully ingests all files in `data/articles/` and `data/wire/` without losing metadata.

#### Task 1.2: Token-Aware Intelligent Chunking (`rag/chunk.py`)
- **GitHub Issue:** [#2](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/2)
- **Status:** **✅ Done (Closed)**
- **Type:** Feature / Foundation
- **Description:** Implement an intelligent document chunker using token-aware recursive character splitting to maintain semantic coherence and prevent mid-word/mid-sentence splits.
- **Key Deliverables:**
  - RecursiveCharacterTextSplitter with tiktoken `cl100k_base` tokenizer.
  - Chunk size (400 tokens) and overlap (50 tokens) configuration.
  - Parent metadata inheritance with `chunk_index` and `chunk_id` tagging (`source::index`).
- **Acceptance Criteria:**
  - [x] Implemented in [`rag/chunk.py`](file:///home/samiran/projects/agentic-ai-learning/daily_planet_ai_desk/rag/chunk.py).
  - [x] Chunks retain lineage to parent documents and avoid truncation of critical context.

#### Task 1.3: Ingestion Quality & Chunking Verification
- **GitHub Issue:** [#3](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/3)
- **Status:** **✅ Done (Closed)**
- **Type:** Verification / Assessment
- **Description:** Validate chunk boundaries against news article paragraphs and complete the Week 1 milestone assessment.
- **Key Deliverables:**
  - 7-question practice quiz on RAG fundamentals, chunking trade-offs, and ingestion pitfalls.
  - Discussion forum participation and weekly live session attendance.
- **Acceptance Criteria:**
  - [x] Passing score on Week 1 assessment; no orphan chunks without metadata.

---

### **Week 2: Embeddings & Vector Search**

#### Task 2.1: OpenAI Embedding Model Benchmarking & Selection
- **GitHub Issue:** [#4](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/4)
- **Status:** **⏳ Todo**
- **Type:** Research & Design
- **Description:** Evaluate OpenAI embedding models (`text-embedding-3-small` vs. `text-embedding-3-large`) on cost, query latency, dimension trade-offs (1536 vs 3072 dims), and semantic retrieval precision.
- **Key Deliverables:**
  - Benchmark report on token costs and embedding generation latency.
  - Final embedding model configuration with cosine similarity distance metric.
- **Acceptance Criteria:**
  - [ ] Justified selection balancing retrieval accuracy and cost for newsroom scale.

#### Task 2.2: Local Semantic Search Engine Prototype (FAISS)
- **GitHub Issue:** [#5](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/5)
- **Status:** **⏳ Todo**
- **Type:** Feature / Prototype
- **Description:** Implement a local vector index using FAISS to build intuition around vector math, cosine distance, and nearest-neighbor search without external infrastructure dependencies.
- **Key Deliverables:**
  - `rag/vector_store.py` with FAISS index creation, vector insertion, and similarity query function.
  - Explainability hook: logging cosine similarity scores and retrieved chunk IDs for any given query.
- **Acceptance Criteria:**
  - [ ] Ability to explain why a chunk was retrieved based on vector similarity score.

#### Task 2.3: Production Vector Database Migration (Qdrant in Docker)
- **GitHub Issue:** [#6](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/6)
- **Status:** **⏳ Todo**
- **Type:** Infrastructure / Production
- **Description:** Migrate vector search from FAISS to a self-hosted Qdrant instance running in Docker, establishing persistent collections, payload filtering, and production API access.
- **Key Deliverables:**
  - `docker-compose.yml` or Docker command for local Qdrant deployment on ports 6333/6334.
  - Qdrant client integration with collection schema and metadata payload indexing.
- **Acceptance Criteria:**
  - [ ] Vector search successfully runs against Qdrant with persistent storage on disk.

#### Task 2.4: Week 2 Assessment & Vector Search Review
- **GitHub Issue:** [#7](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/7)
- **Status:** **⏳ Todo**
- **Type:** Verification / Assessment
- **Description:** Complete the 7-question practice quiz on embedding dimensions, distance metrics, and vector database trade-offs; participate in live session.
- **Acceptance Criteria:**
  - [ ] Passing score on Week 2 quiz; peer review of Qdrant migration.

---

### **Week 3: Retrieval Strategies: Hybrid Search & Reranking**

#### Task 3.1: Sparse Keyword Search Engine (BM25)
- **GitHub Issue:** [#8](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/8)
- **Status:** **⏳ Todo**
- **Type:** Feature
- **Description:** Build a BM25 inverted index over chunked news articles to solve the exact-match problem (names, tickers, dates, wire codes) that dense vector search misses.
- **Key Deliverables:**
  - Inverted index construction using rank-bm25 or equivalent.
  - Keyword search function returning top-k matching chunks with BM25 scores.
- **Acceptance Criteria:**
  - [ ] Exact term queries (e.g. specific journalist names or article IDs) return 100% precision.

#### Task 3.2: Hybrid Retrieval & Score Fusion
- **GitHub Issue:** [#9](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/9)
- **Status:** **⏳ Todo**
- **Type:** Feature
- **Description:** Combine dense vector search results from Qdrant with sparse BM25 keyword search results into a unified hybrid retriever.
- **Key Deliverables:**
  - Reciprocal Rank Fusion (RRF) algorithm implementation.
  - Configurable weighting between dense and sparse scores.
- **Acceptance Criteria:**
  - [ ] Hybrid search out-retrieves pure vector and pure keyword search on mixed query types.

#### Task 3.3: Two-Stage Retrieval with Cross-Encoder / Cohere Reranking
- **GitHub Issue:** [#10](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/10)
- **Status:** **⏳ Todo**
- **Type:** Optimization
- **Description:** Add a second-stage reranker (Cross-Encoder or Cohere Rerank) that re-scores the combined candidate pool to place the most accurate chunks at the top.
- **Key Deliverables:**
  - "Retrieve-broad-then-rerank-precise" architecture (e.g., retrieve top-25 $\rightarrow$ rerank to top-3).
  - Top-k parameter tuning and qualitative evaluation of ranking improvement.
- **Acceptance Criteria:**
  - [ ] High-relevance chunks consistently rank at position 1 or 2 after reranking.

#### Task 3.4: Week 3 Assessment & Retrieval Review
- **GitHub Issue:** [#11](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/11)
- **Status:** **⏳ Todo**
- **Type:** Verification / Assessment
- **Description:** Complete the 7-question practice quiz on BM25, RRF score fusion, and reranking architectures; participate in live session.
- **Acceptance Criteria:**
  - [ ] Passing score on Week 3 quiz.

---

### **Week 4: Grounded Generation: Citations & Honest Fallbacks**

#### Task 4.1: Context-Constrained Prompt Engineering
- **GitHub Issue:** [#12](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/12)
- **Status:** **⏳ Todo**
- **Type:** Feature
- **Description:** Design generation prompt templates that instruct the LLM to formulate responses strictly from the retrieved chunks and prohibit ungrounded external assumptions.
- **Key Deliverables:**
  - Structured prompt templates in `rag/generate.py`.
  - Context framing with clear separation between user query and reference documents.
- **Acceptance Criteria:**
  - [ ] Model refuses to answer queries when information is intentionally omitted from context.

#### Task 4.2: Inline Citation & Source Attribution Engine
- **GitHub Issue:** [#13](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/13)
- **Status:** **⏳ Todo**
- **Type:** Feature
- **Description:** Implement strict source attribution where every factual claim in the generated answer includes an inline citation referencing the source chunk ID.
- **Key Deliverables:**
  - Citation parsing and formatting (e.g., `[article_1.md::chunk_0]`).
  - Validation utility to verify that cited sources exist in the retrieved context pool.
- **Acceptance Criteria:**
  - [ ] 100% of factual assertions cite corresponding source chunk IDs.

#### Task 4.3: Low-Confidence Detection & Honest Fallbacks
- **GitHub Issue:** [#14](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/14)
- **Status:** **⏳ Todo**
- **Type:** Safety & Robustness
- **Description:** Implement confidence heuristics and prompt refusal triggers when retrieved chunks have low similarity or lack answers.
- **Key Deliverables:**
  - Minimum similarity threshold check before invoking generation.
  - Fallback message: *"I cannot verify that from the available archive."*
- **Acceptance Criteria:**
  - [ ] Zero hallucinations on unanswerable test queries; correct refusals generated reliably.

#### Task 4.4: Unified RAG Pipeline Endpoint (`rag/pipeline.py`)
- **GitHub Issue:** [#15](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/15)
- **Status:** **⏳ Todo**
- **Type:** Architecture
- **Description:** Wrap the entire flow (`Retrieve -> Rerank -> Generate -> Cite/Refuse`) into a clean, reusable Python service and API endpoint.
- **Key Deliverables:**
  - `query_rag(query: str, top_k: int) -> RAGResponse` entrypoint.
- **Acceptance Criteria:**
  - [ ] End-to-end execution completes synchronously with clean response objects.

#### Task 4.5: Week 4 Assessment & Generation Review
- **GitHub Issue:** [#16](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/16)
- **Status:** **⏳ Todo**
- **Type:** Verification / Assessment
- **Description:** Complete the 7-question practice quiz on grounded generation, citations, and hallucination prevention; attend live session.
- **Acceptance Criteria:**
  - [ ] Passing score on Week 4 quiz.

---

### **Week 5: Access Control, Data Freshness & The Managed Path**

#### Task 5.1: Retrieval-Layer Access Control via Metadata Filtering
- **GitHub Issue:** [#17](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/17)
- **Status:** **⏳ Todo**
- **Type:** Security
- **Description:** Implement role-based access control (RBAC) at the vector search layer to ensure users can only retrieve chunks from sections/publications they are authorized to view.
- **Key Deliverables:**
  - Metadata filtering in Qdrant based on user roles (e.g., `role: editorial`, `publication: MetroWire`).
  - Security audit demonstrating that unauthorized chunks are filtered before prompt assembly.
- **Acceptance Criteria:**
  - [ ] Complete data isolation: unauthorized documents never reach the LLM context.

#### Task 5.2: Incremental Re-Indexing & Data Freshness Strategy
- **GitHub Issue:** [#18](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/18)
- **Status:** **⏳ Todo**
- **Type:** Feature / Data Engineering
- **Description:** Build an incremental indexing pipeline that detects added, updated, or deleted articles in the newsroom archive and updates both Qdrant and BM25 without full rebuilds.
- **Key Deliverables:**
  - Change detection module tracking file hashes and timestamps.
  - Atomic upsert and deletion handlers in `rag/reindex.py`.
- **Acceptance Criteria:**
  - [ ] Newly published articles become searchable within seconds of arrival.

#### Task 5.3: GraphRAG Conceptual Architecture Analysis
- **GitHub Issue:** [#19](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/19)
- **Status:** **⏳ Todo**
- **Type:** Research
- **Description:** Formulate an architectural study of GraphRAG (knowledge graphs + vector retrieval) for complex investigative news queries involving multi-hop entity relationships.
- **Key Deliverables:**
  - Architectural brief identifying newsroom queries best suited for GraphRAG vs vector RAG.
- **Acceptance Criteria:**
  - [ ] Clear identification of trade-offs in complexity, cost, and query capability.

#### Task 5.4: Build vs. Buy Architectural Evaluation
- **GitHub Issue:** [#20](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/20)
- **Status:** **⏳ Todo**
- **Type:** Architecture & Strategy
- **Description:** Conduct a structured trade-off analysis comparing the self-built RAG pipeline (Qdrant + self-hosted components) against managed solutions (Pinecone, AWS Bedrock Knowledge Bases, AWS OpenSearch Service).
- **Key Deliverables:**
  - Trade-off matrix covering cost, operational overhead, vendor lock-in, data sovereignty, and customization flexibility.
- **Acceptance Criteria:**
  - [ ] Reasoned build-vs-buy recommendation for an enterprise newsroom.

#### Task 5.5: Week 5 Assessment & Access Control Review
- **GitHub Issue:** [#21](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/21)
- **Status:** **⏳ Todo**
- **Type:** Verification / Assessment
- **Description:** Complete the 7-question practice quiz on retrieval access control, data freshness, and managed cloud RAG; attend live session.
- **Acceptance Criteria:**
  - [ ] Passing score on Week 5 quiz.

---

### **Week 6: Retrieval Evaluation & Production RAG Capstone**

#### Task 6.1: Golden Evaluation Dataset Construction
- **GitHub Issue:** [#22](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/22)
- **Status:** **⏳ Todo**
- **Type:** Testing & Evaluation
- **Description:** Curate a golden benchmark dataset comprising newsroom queries, expected source chunks, and target verified answers.
- **Key Deliverables:**
  - `evaluation/golden_dataset.json` containing 30+ diverse journalistic query scenarios (fact lookup, multi-document synthesis, and unanswerable queries).
- **Acceptance Criteria:**
  - [ ] Golden dataset peer-reviewed and frozen for baseline evaluation.

#### Task 6.2: Quantitative Retrieval Metrics Evaluation
- **GitHub Issue:** [#23](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/23)
- **Status:** **⏳ Todo**
- **Type:** Testing & Evaluation
- **Description:** Build an evaluation harness to compute statistical retrieval metrics over the golden dataset.
- **Key Deliverables:**
  - Automated calculation of **Hit-Rate@K** and **Mean Reciprocal Rank (MRR)**.
  - Comparative evaluation scripts comparing pure vector vs hybrid vs reranked pipelines.
- **Acceptance Criteria:**
  - [ ] Retrieval metrics exported to CSV/JSON reports.

#### Task 6.3: Answer Faithfulness & LLM-as-a-Judge Evaluation
- **GitHub Issue:** [#24](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/24)
- **Status:** **⏳ Todo**
- **Type:** Testing & Evaluation
- **Description:** Implement an automated LLM-as-a-judge harness to evaluate generation quality, measuring Faithfulness (hallucination rate) and Answer Relevance.
- **Key Deliverables:**
  - Judge prompts and scoring rubric in `rag/evaluate.py`.
  - Detection of unfaithful assertions and misattributed citations.
- **Acceptance Criteria:**
  - [ ] Faithfulness score > 95% on golden benchmark questions.

#### Task 6.4: Observability & Tracing with Local Langfuse
- **GitHub Issue:** [#25](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/25)
- **Status:** **⏳ Todo**
- **Type:** Infrastructure / Observability
- **Description:** Connect the RAG pipeline to a local Langfuse instance to trace every query execution, capturing latency breakdowns, token consumption, and per-query costs.
- **Key Deliverables:**
  - Langfuse tracer integration decorating retrieval, reranking, and generation spans.
  - Dashboard configuration displaying latency percentiles (p50, p95) and token expenditures.
- **Acceptance Criteria:**
  - [ ] End-to-end request traces viewable in Langfuse UI.

#### Task 6.5: Automated Regression Testing & Capstone Synthesis
- **GitHub Issue:** [#26](https://github.com/SamiranB8888/Daily_Planet_AI_Desk/issues/26)
- **Status:** **⏳ Todo**
- **Type:** Quality Assurance & Capstone Deliverable
- **Description:** Build a CI/CD regression check that flags any pipeline modification degrading retrieval hit-rate or faithfulness, and assemble the final Capstone package.
- **Key Deliverables:**
  - Regression test suite (`pytest -m regression`).
  - Final Module 2 Capstone demonstration and documentation package.
  - Final 7-question practice quiz and capstone presentation.
- **Acceptance Criteria:**
  - [ ] Passing automated regression checks; complete production-ready pipeline deliverable.
