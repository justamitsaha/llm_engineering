# Encoder Models vs. Vector Databases

## 1\. From Document Splitting to Embeddings

The previous step in the RAG pipeline was **document splitting**.

We took large documents and divided them into smaller **chunks** so that individual pieces would be more likely to correspond to useful questions.

A **Recursive Character Text Splitter** was used to intelligently split text:

```
Large Document
      ↓
Try double newlines
      ↓
Try single newlines
      ↓
Try spaces
      ↓
~1000-character chunks
      +
~200-character overlap
```

The next step is to turn these chunks into **vectors** and store them.

* * *

# 2\. Two Completely Different Steps

A very important distinction:

> **An encoder creates the vector. A vector database stores and searches the vector.**

These are separate components.

```
Text
  ↓
Encoder / Embedding Model
  ↓
Vector
  ↓
Vector Database
  ↓
Fast similarity search
```

The **vector database does NOT create the vector**.

The encoder/embedding model creates it.

* * *

## 3\. What Does an Encoder Do?

An **encoder/embedding model** takes text and converts it into a collection of numbers representing its meaning.

For example:

```
"Ticket prices to London"
          ↓
   Embedding Model
          ↓
[0.12, -0.45, 0.81, ...]
```

This numerical representation is the **vector embedding**.

The purpose is to make it possible to compare pieces of text based on their **semantic similarity**.

* * *

# 4\. What Does a Vector Database Do?

Once the encoder has created vectors, those vectors can be stored in a **vector database/vector store**.

A vector database is optimized for questions such as:

> **"Which stored vector is closest to this vector?"**

Rather than traditional database queries such as:

```
SELECT * FROM ...
```

it performs **vector similarity searches**.

Conceptually:

```
Query Vector
     ↓
Vector Database
     ↓
Find nearby/similar vectors
     ↓
Return associated text
```

The database's job is therefore primarily **storage + fast vector retrieval**.

* * *

# 5\. Encoder vs. Vector Database

This distinction is easy to lose because libraries such as LangChain may let you create and associate them together in code.

Keep the conceptual separation clear:

| Component | Main job |
| --- | --- |
| **Encoder / Embedding Model** | Converts text → vectors |
| **Vector Database** | Stores vectors + retrieves similar vectors |

Think of it like:

```
Encoder = creates the coordinates

Vector Database = stores the coordinates
                  and finds nearby coordinates
```

* * *

# 6\. Popular Encoder / Embedding Models

The lecture mentions several important examples.

### Word2Vec

One of the early approaches.

It could convert individual words into vectors.

It is historically associated with relationships such as:

```
KING - MAN + WOMAN ≈ QUEEN
```

It is mainly important historically in this context.

### BERT

Released by Google in **2018**, BERT was a powerful Transformer-based encoder and remains relevant in some applications.

### OpenAI Embeddings

Examples mentioned:

-   **text-embedding-3-small**
-   **text-embedding-3-large**

### Google's Embedding Model

-   **Gemini Embedding 001**

### Hugging Face / Sentence Transformers

There are many open-source embedding models.

One particularly popular model mentioned is:

-   **all-MiniLM-L6-v2**

* * *

# 7\. The RAG Pipeline So Far

The complete process discussed can now be viewed as:

```
Documents
    ↓
Document Loader
    ↓
Text Splitter
    ↓
Chunks
    ↓
Embedding Model
    ↓
Vectors
    ↓
Vector Store
    ↓
Fast similarity retrieval
```

For example:

```
"Ticket prices to London"
          ↓
   Embedding Model
          ↓
     Vector A
          ↓
   Chroma / Vector Store
```

Later, a user's question can also be converted into a vector and compared against these stored vectors.

* * *

# 8\. Which Embedding Model Should You Choose?

This is one of the most important practical points from the lecture.

Different embedding models can perform **very differently for different use cases**.

Therefore, choosing the embedding model requires:

-   experimentation
-   testing
-   evaluating retrieval quality
-   understanding your particular business problem

The important question is:

> **Which embedding model retrieves the most relevant content for my particular application?**

There isn't necessarily one universally "best" embedding model.

* * *

# 9\. Which Vector Database Should You Choose?

The choice of vector database is somewhat different.

The lecture emphasizes that this is more like a **traditional infrastructure/database decision**.

Important considerations include:

-   **Cost**
-   **Performance**
-   **Scalability**
-   **Resiliency**
-   **Backups**
-   **Security**

So the distinction is:

```
Embedding Model
       ↓
AI/ML decision
"What representation gives me
the best retrieval?"

Vector Database
       ↓
Infrastructure decision
"Where should I store/search it,
and what are the cost/performance
requirements?"
```

* * *

# 10\. Popular Vector Stores

### Chroma

The course uses **Chroma**.

Advantages mentioned:

-   Open source
-   Easy to use
-   Free

### FAISS

**FAISS (Facebook AI Similarity Search)** is another very popular open-source option.

The lecture emphasizes that FAISS is not really a conventional database; it is primarily an **in-memory vector search system**.

Its purpose is to efficiently search through vectors in memory.

### Pinecone and Weaviate

These are examples of **paid/managed vector database offerings**.

They can be useful when you need more scalable, managed infrastructure, particularly for enterprise applications.

* * *

# 11\. You Don't Always Need a Dedicated Vector Database

A major modern development is that traditional databases increasingly support vectors **natively**.

Examples mentioned include:

```
PostgreSQL
MongoDB
Elasticsearch
```

These systems can store and search vector data alongside their existing database/search capabilities.

For example:

```
Traditional Database
        +
Vector Storage
        +
Vector Similarity Search
```

Therefore, you don't necessarily have to introduce a separate dedicated vector database.

* * *

# 12\. Elasticsearch Example

The lecturer gives a real-world example involving **Elasticsearch**.

It can natively support vector search, allowing very large numbers of vectors to be searched quickly.

The example mentioned involves approximately:

```
200 million vectors
```

with searches that combine filtering and vector similarity and can execute in only a few seconds.

This illustrates that mainstream high-performance search/database platforms can now handle vector workloads directly.

* * *

# 13\. The Main Decision: Encoder First

The strongest message from the lecture is:

> **As an AI engineer, spend more effort choosing and testing the embedding model than worrying about which vector database to use.**

Why?

Because the embedding model determines **how your text is represented**.

If the representation is poor:

```
Poor Encoder
     ↓
Poor Vectors
     ↓
Poor Similarity Search
     ↓
Poor Retrieved Context
     ↓
Poor RAG Results
```

A good vector database cannot completely solve a poor embedding representation.

On the other hand:

```
Good Encoder
     ↓
Useful Vectors
     ↓
Relevant Retrieval
     ↓
Better Context
     ↓
Better RAG Results
```

* * *

# 14\. A Useful Mental Model

Think of the two components like this:

### Encoder

Answers:

> **"How should I represent this text numerically so that meaning and similarity are captured?"**

### Vector Database

Answers:

> **"How can I efficiently store these vectors and find the ones most similar to this query?"**

So:

```
             ENCODER
        ┌─────────────────┐
Text →  │ Meaning → Vector│
        └────────┬────────┘
                 ↓
              Vector
                 ↓
        ┌─────────────────┐
        │  VECTOR STORE   │
        │                 │
        │ Store + Search  │
        └─────────────────┘
```

* * *

# 15\. Important Qualification

The lecture notes that the distinction isn't absolute.

Some sophisticated vector/search platforms can provide specialized similarity-search capabilities and performance optimizations.

So there **can** be meaningful differences between vector stores beyond basic infrastructure.

However, the main practical message remains:

> **Most of the time, the embedding model deserves more attention because it directly affects the relevance of retrieved content.**

* * *

# Key Takeaways

1.  **Document splitting** produces manageable chunks of text.
2.  An **encoder/embedding model** converts those chunks into vectors.
3.  A **vector database/store** stores those vectors and performs similarity searches.
4.  **The encoder creates vectors; the vector database does not.**
5.  Different embedding models can perform very differently depending on the use case.
6.  Choosing an embedding model requires **testing and experimentation**.
7.  Examples of embedding models include **Word2Vec, BERT, OpenAI embeddings, Gemini Embedding 001, and all-MiniLM-L6-v2**.
8.  Examples of vector stores include **Chroma, FAISS, Pinecone, and Weaviate**.
9.  Traditional databases such as **PostgreSQL, MongoDB, and Elasticsearch** can also support vectors natively.
10.  Vector-store selection is largely an **infrastructure decision** involving cost, performance, scalability, security, backups, and resiliency.
11.  For RAG quality, the **embedding model is usually the more important AI engineering decision**.

## One-line memory aid

**Encoder = creates vectors; vector database = stores and searches vectors—so focus on the encoder for semantic quality and the database for infrastructure needs.**