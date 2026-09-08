Week 1
Why RAG, Ingestion and Chunking
Opens the module by ingesting messy real-world documents and chunking them intelligently, the foundation that decides whether RAG works at all.
Objective:
1. Explain what RAG is and why it beats fine-tuning and long-context approaches for grounded, up to date, source-attributable knowledge.
2. Describe the full RAG pipeline, ingest, chunk, embed, store, retrieve, generate, and the role of each stage.
3. Load documents from PDF, HTML and markdown using LangChain loaders, handling real-world messiness.
4. Compare chunking strategies, fixed, recursive, semantic, and justify chunk size and overlap as a driver of retrieval quality.
5. Explain why poor chunking is the most common hidden cause of bad RAG answers.


Week 2
Embeddings and Vector Search
Makes the archive searchable by meaning, building intuition with FAISS before moving to Qdrant at production shape.
Objective:
1. Explain what a text embedding is and how cosine similarity measures semantic closeness.
2. Select an OpenAI embedding model, text-embedding-3-small versus text-embedding-3-large, on a cost-versus-quality basis, including the dimensions trade-off.
3. Build semantic search over the corpus with FAISS locally, with nothing hidden.
4. Migrate the same search to Qdrant, running locally in Docker, and justify why a dedicated vector database is needed past a prototype.
5. Explain, at a query, exactly why a given chunk was retrieved.


Week 3

Retrieval Strategies: Hybrid Search and Reranking
Combines keyword and vector search into a hybrid retriever, then adds reranking so retrieval becomes trustworthy, not only plausible.
Objective:
1. Explain why pure vector search fails on exact-match terms, and why a newsroom cannot tolerate that.
2. Implement BM25 keyword retrieval and describe what it captures that embeddings miss.
3. Combine keyword and vector retrieval into a hybrid retriever, and reason about score fusion.
4. Add a reranking stage, cross-encoder or Cohere Rerank, and explain retrieve-broad-then-rerank-precise.
5. Tune top-k and qualitatively evaluate whether retrieval improved.

Week 4

Grounded Generation: Citations and Honest Fallbacks
Closes the loop by turning retrieved chunks into a grounded, cited answer, and one that knows when to say it cannot verify something.
Objective:
1. Assemble retrieved context into a generation prompt that produces a grounded answer with inline citations.
2. Design prompt patterns that force answering only from the provided context, and attribute each claim.
3. Detect low-confidence retrieval and implement an honest "I cannot verify that" fallback instead of hallucinating.
4. Articulate why, in high-stakes domains, a correct refusal beats a plausible guess.
5. Expose the full retrieve, rerank, generate flow as a single RAG endpoint.

Week 5

Access Control, Data Freshness and the Managed Path
Adds per-user access control and data freshness, then compares the self-built pipeline against the managed path.
Objective:
1. Implement per-user access control in retrieval through metadata filtering.
2. Explain why access control belongs at the retrieval layer, and the privacy risk of ignoring it.
3. Design a data-freshness and re-indexing strategy for a continuously changing corpus.
4. Explain GraphRAG at a concept level and identify the questions it suits.
5. Compare self-built RAG against managed options, Pinecone, AWS Bedrock Knowledge Bases, AWS OpenSearch Service, and make a reasoned build-versus-buy call.

Week 6

Retrieval Evaluation and the Production RAG Capstone
Builds an evaluation harness for the pipeline, then assembles every stage into the complete production-minded RAG system.
Objective:
1. Build a golden evaluation set, questions plus expected sources and answers, for a RAG system.
2. Measure retrieval quality, hit-rate and MRR, and answer quality, faithfulness via LLM-as-judge.
3. Use Langfuse, running locally, to trace and observe RAG requests: chunks, latency, cost per query.
4. Run a regression check that catches when a change degrades retrieval or faithfulness.
5. Assemble and present the complete production-minded RAG pipeline as the Module 2 deliverable.

