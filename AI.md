# ChromaDB Architecture & Technical Overview

**ChromaDB** is an open-source vector database designed by **Jeff Huber** and **Anton Troynikov** (founded in 2022). It provides AI applications and Large Language Models (LLMs) with long-term memory via semantic storage.

---

## 1. Internal Architecture & Components
ChromaDB avoids reinventing the wheel by combining two highly efficient storage and index engines:

*   **The Metadata Layer (`chroma.sqlite3`):** An embedded SQLite database that tracks collection names, document UUIDs, schemas, configurations, and textual metadata.
*   **The Vector Index (`hnswlib`):** A high-performance C++ library that stores raw floating-point vector arrays and computes similarity algorithms.
*   **Data Transfer Engine (Apache Arrow):** Utilizes an in-memory columnar data format to enable high-speed data exchanges between applications and the database.

---

## 2. Data Structures Used
ChromaDB organizes information using distinct data structures tailored for performance:

*   **Hierarchical Navigable Small World (HNSW) Graphs:** A multi-layered graph data structure used to index vectors. The top layers contain sparse data points for fast, macroscopic jumping; lower layers contain highly dense data networks for granular, precise matching.
*   **B-Trees / Relational Tables:** Used by SQLite to maintain strict relational mapping for strict operations (ID indexing, text lookups, and operational updates).

---

## 3. How It Works (Pipeline)

```text
[Raw Text/Image] ──► [Embedding Model] ──► [ChromaDB (Stores Vector in HNSW Graph)]
                                                        │ (Matches mathematically)
[User Query]     ──► [Embedding Model] ──► [Query Vector ┘]
```

1.  **Embedding Generation:** Raw texts are passed through an embedding model (OpenAI, Hugging Face, or native Sentence Transformers) to convert data into high-dimensional vector arrays.
2.  **Graph Indexing:** The resulting vectors are mapped into the HNSW graph based on geometrical closeness, while the raw texts and properties are stored in the SQLite schema layer.
3.  **Vector Querying:** Incoming search queries are converted into query vectors. ChromaDB jumps across the HNSW graph layers to locate the mathematically closest match utilizing **Cosine** or **L2 Euclidean distance** metrics.
4.  **Data Hydration:** Matches are cross-referenced with SQLite to retrieve the original, human-readable strings and attributes before returning them to the user.

---

## 4. Key Use Cases

*   **Retrieval-Augmented Generation (RAG):** Supplementing LLMs with custom, dynamic context to eliminate AI hallucinations.
*   **Semantic Search Engine:** Powering discovery engines that match textual intent and context rather than exact keyword strings.
*   **AI Agent Context Memory:** Allowing autonomous agents to log historical runtime states and retrieve past conversations.
