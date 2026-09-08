Week 1  ·  Why RAG, Ingestion and Chunking
What you will build: The first stage of the Daily Planet RAG pipeline, a rag module that loads the newsroom corpus from PDF, HTML and markdown, and splits it into well-formed chunks.

What this contains:

Topics: why RAG beats fine-tuning and long-context, the six-stage RAG pipeline, document ingestion, chunking strategies.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: explain why RAG is the practical choice for a constantly changing archive, and chunk real documents so retrieval has something useful to work with.

Week 2  ·  Embeddings and Vector Search
What you will build: Semantic search over the chunked archive, first with FAISS locally, then migrated to Qdrant.

What this contains:

Topics: embeddings and cosine similarity, choosing an embedding model, vector databases, FAISS to Qdrant.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: explain, for a given query, exactly why a chunk was retrieved, and justify moving from a local library to a dedicated vector database.

Week 3  ·  Retrieval Strategies: Hybrid Search and Reranking
What you will build: A hybrid retriever that combines BM25 keyword search with vector search, then a reranking stage on top of it.

What this contains:

Topics: the exact-match problem, BM25, hybrid search, reranking.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: explain why vector search alone misses exact terms, and combine it with keyword search and reranking so the right answer reaches the top.

Week 4  ·  Grounded Generation: Citations and Honest Fallbacks
What you will build: The answering stage, a single RAG endpoint that generates a grounded, cited answer, or an honest refusal when the evidence is not there.

What this contains:

Topics: grounded generation, citations, the honest fallback, assembling the RAG endpoint.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: design a prompt that answers only from retrieved context, and explain why a correct refusal beats a plausible guess.

Week 5  ·  Access Control, Data Freshness and the Managed Path
What you will build: Per-user access control and incremental re-indexing on the self-built pipeline, plus a hands-on comparison against the managed path.

What this contains:

Topics: access control at the retrieval layer, data freshness, GraphRAG at a concept level, build versus buy.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: filter retrieval by role, design a re-indexing strategy, and make a reasoned build-versus-buy call against Pinecone and AWS Bedrock Knowledge Bases.

Week 6  ·  Retrieval Evaluation and the Production RAG Capstone
What you will build: An evaluation harness for the pipeline, and the final assembly of every stage into one production-minded RAG system.

What this contains:

Topics: golden evaluation sets, retrieval and faithfulness metrics, observability with Langfuse, module synthesis.
Assessment: a seven-question practice quiz, a discussion forum, and a live session.
By the end you will be able to: measure whether a change improved or degraded the system, and present the complete pipeline as the Module 2 deliverable.