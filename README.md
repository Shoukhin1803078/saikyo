# saikyo
# saikyo



The client is clearly requesting a NotebookLM-style AI search experience, which technically translates to a RAG-based semantic search system over the internal job database, where AI understands user intent, restricts search to the job dataset and returns highly relevant job posts instead of keyword-based results.



How RAG works?

1️⃣ Data Sources
-----------------
- Database (SQL / NoSQL)
- Files (CSV, XLSX, PDF, Docs)
- APIs / Web
      ↓

2️⃣ Data Ingestion (ETL)
-----------------
- Extract raw data
- Clean & normalize text
- Optional: merge relevant fields
      ↓

3️⃣ Chunking / Preprocessing
-----------------
- Split long documents/text into smaller chunks
- Add metadata (e.g., author, date, category, location)
- Optional: remove stopwords / noise
      ↓

4️⃣ Embedding
-----------------
- Convert text chunks → vector embeddings
- Models: OpenAI, Cohere, Vertex AI, Bedrock, etc.
      ↓

5️⃣ Vector Store / Index
-----------------
- Store embeddings + metadata in a vector DB
- Vector DB options: Pinecone, Weaviate, FAISS, Milvus, ChromaDB
- Build indexes for fast similarity search
      ↓

6️⃣ Query / Retrieval
-----------------
- User submits query (text)
- Convert query → embedding
- Search vector DB for top-k similar chunks
      ↓

7️⃣ Optional Filtering
-----------------
- Use metadata filters for precision
  e.g., location = Dhaka, type = remote, category = engineering
      ↓

8️⃣ Optional Reranking
-----------------
- Use a model to re-rank top results for better relevance
- Models: Cohere Rerank, OpenAI GPT, custom scoring
      ↓

9️⃣ Optional LLM / Summarization
-----------------
- Combine retrieved chunks
- Generate summary, structured output, or direct answer
- Makes results human-readable
      ↓

🔟 Return Results
-----------------
- Show final answer / recommended items / jobs / insights
- Can include confidence score, highlights, or metadata



For this project the pipeline should be like this:


Step-1(Data preprocessing):
DB (Job Table)
      ↓  Export
XLSX File (title, description, location, type, etc.)
      ↓  Load into Python / ETL script
Read XLSX → Pandas DataFrame
      ↓  Preprocessing
- Clean text
- Combine columns for embedding (title + description + type + location)

Step-2(RAG pipeline build):
- Optional: chunk long descriptions
      ↓  Embedding
- Call embedding model (OpenAI, Cohere, Vertex AI, Bedrock)
- Get vector representation for each job
      ↓  Vector DB Ingestion
- Store vectors in vector DB (Weaviate / Pinecone / FAISS / Milvus)
- Save metadata (location, type, job_id) for filtering
      ↓  Indexing & Ready
- All jobs are now searchable by vector similarity
- Metadata filters available for refined search


Step-3(Inferencing):

User Query
     ↓
Embed Query
     ↓
Vector Search (Job Dataset)
     ↓
Filter & Rank (metadata: location, type)
     ↓
Optional LLM Answer / Summary
     ↓
Return Top Jobs




Embedding Models with cost :


| Provider / Model                  | Quality              | Cost                           | Best For                              |
| --------------------------------- | -------------------- | ------------------------------ | ------------------------------------- |
| **SBERT / Sentence-Transformers** | Good                 | Free (infra cost)           | Self-hosted RAG, prototypes           |
| **HuggingFace Embedding Models**  | Good                 | Free (infra cost)           | Custom / multilingual embeddings      |
| **OpenAI text‑embedding‑3‑small** | Good                 | Lowest (~$0.02/1M tokens)   | Cheap RAG jobs search                 |
| **OpenAI text‑embedding‑3‑large** | Better accuracy      | Medium (~$0.13/1M tokens) | High-precision semantic search        |
| **Cohere Embed v3**               | Balanced             | Variable                  | Multi-language / hybrid search        |
| **Google Vertex AI**              | Good                 | Competitive               | GCP integrated RAG                    |
| **AWS Bedrock embeddings**        | Depends on model     | Variable                  | AWS ecosystem apps                    |
| **LLaMA / Vicuna Embedding**      | High (large context) | Free (infra cost)           | Advanced self-hosted RAG / enterprise |



Opeai Embedding model cost:

| Model                      | Typical Use                                       | Cost (est)                                             |
| -------------------------- | ------------------------------------------------- | ------------------------------------------------------ |
| **text‑embedding‑3‑small** | Cost‑efficient, good for embedding jobs & queries | **~$0.02 per 1M tokens**         |
| **text‑embedding‑3‑large** | Higher quality retrieval                          | **~$0.13 per 1M tokens**       |
| (Older **ada‑002**)        | Legacy, less                           | ~$0.10 per 1M tokens  |





Vector DB:

| DB       | Easy | Scale | Hybrid Search | Managed |
| -------- | ---- | ----- | ------------- | ------- |
| FAISS    | Yes   | No    | No             | No       |
| ChromaDB | Yes    | No     | No             | No       |
| Pinecone | Yes    | Yes     | Limited            | Yes       |
| Weaviate | No   | Yes     | Yes             | Yes       |
| Milvus   | No    | Yes     | Limited            | Limit      |



Vector DB Costing:

| DB           | Easy | Scale | Hybrid Search | Managed | Cost Estimate (Monthly)                    | Best For                              |
| ------------ | ---- | ----- | ------------- | ------- | ------------------------------------------ | ------------------------------------- |
| **FAISS**    | Yes  | No    | No            | No      | Free (self-hosted, infra cost $20–$200) | Local RAG / experiments               |
| **ChromaDB** | Yes  | No    | No            | No      | Free (self-hosted, infra cost $15–$120) | Small RAG, prototypes                 |
| **Pinecone** | Yes  | Yes   | Limited       | Yes     | $50–$800+                               | Production RAG, scalable apps         |
| **Weaviate** | No   | Yes   | Yes           | Yes     | $25–$400+                               | Hybrid search / semantic jobs search  |
| **Milvus**   | No   | Yes   | Limited       | Limited | $50–$600+                               | Enterprise-scale RAG / large datasets |




FAISS & ChromaDB: free to use, just pay for hosting (CPU/GPU).
Pinecone & Weaviate: managed, easier to scale, cost depends on vectors & query volume.
Milvus: great for massive datasets, but setup can be complex and infra-heavy.


RAG Third-party services:

- AWS OpenSearch:
    - managed search with vector + keyword combined (hybrid)
    - scale to billions of vectors

- Google Vertex AI Search:
    - index data and search it semantically
    - semantic/hybrid search and RAG


Opensource Vector DB:
- ChromaDB : 
    - lightweight
    - self hosted
    - No metadata filtering
    - No hybrid search 
    
- FAISS (Meta):
    - Super fast similarity search
    - Free & open-source
    - No metadata filtering
    - No hybrid search 

- Pinecone:
    - Fully managed (zero ops)
    - Scales insanely well
    - High availability

- Weaviate:
    - Native hybrid search (vector + keyword)
    - Strong metadata filtering
    - Open-source + managed option






