# RAG: How Vector Search Finds Relevant Context

## 1\. The Big Idea Behind RAG

**RAG (Retrieval-Augmented Generation)** works by combining:

1.  A user's **question**
2.  A **knowledge base/database**
3.  A **vector/embedding model** to find relevant information
4.  An **autoregressive LLM** to generate the final answer

The key idea is:

```
User Question
      ↓
Convert question into a vector
      ↓
Search Vector Data Store
      ↓
Retrieve semantically relevant text
      ↓
Put text into LLM prompt
      ↓
Autoregressive LLM generates answer
```

* * *

## 2\. Why Ordinary Keyword Search Isn't Enough

Suppose the knowledge base contains:

> "Ticket prices to London"

But the user asks:

> "What are ticket prices to Heathrow?"

A simple text/string search might fail because the word **"Heathrow"** isn't present in the database.

However, Heathrow is an airport in London, so the two questions have **similar meaning**.

RAG solves this using **embeddings/vectors**.

* * *

## 3\. Turning the User's Question Into a Vector

First, an **encoder/embedding model** converts the user's question into a vector.

For example:

```
"What are ticket prices to Heathrow?"
                ↓
       Encoder / Embedding Model
                ↓
       [0.21, -0.73, 0.45, ...]
```

This vector represents the **meaning** of the question rather than simply its exact words.

* * *

## 4\. Turning the Knowledge Base Into Vectors

The same encoder is also used beforehand to convert the information in the knowledge base into vectors.

Conceptually:

```
Knowledge Base

"Ticket prices to London" ──→ Vector A
"Hotels in Paris"          ──→ Vector B
"Weather in New York"      ──→ Vector C
             ...
```

The system stores both:

-   the **original text**
-   its corresponding **vector**

This collection is called a **Vector Data Store** or **Vector Store**.

```
Vector Data Store
┌───────────────────────────────┐
│ Text + Vector                 │
│                               │
│ Ticket prices to London       │ → Vector A
│ Hotels in Paris               │ → Vector B
│ Weather in New York           │ → Vector C
└───────────────────────────────┘
```

* * *

## 5\. The "Fuzzy Lookup"

When the user asks:

> "What are ticket prices to Heathrow?"

the system converts that question into a vector and asks the vector store:

> **"What information has a meaning closest to this question?"**

It isn't looking for an exact string match.

Instead:

```
User Question
"What are ticket prices to Heathrow?"
             ↓
          Vector Q
             ↓
     Find nearby vectors
             ↓
"Ticket prices to London"
             ↓
       Relevant context
```

Because **Heathrow → London** is semantically related, the London ticket-price information can be retrieved even though the exact word _Heathrow_ may not exist in the stored text.

This is the **fuzzy/semantic lookup** idea.

* * *

## 6\. What Actually Gets Sent to the LLM?

This is a **very important distinction**.

There are two different models involved:

### Encoder / Embedding Model

Used for:

```
Question → Vector
Database text → Vectors
```

Its job is to help **find relevant information**.

### Autoregressive LLM

Used for:

```
Question + Retrieved Context → Answer
```

The autoregressive LLM does **not** receive the vectors.

It receives **natural language**.

For example:

```
User question:
"What are ticket prices to Heathrow?"

Retrieved context:
"Ticket prices to London ..."

                ↓

       Autoregressive LLM
                ↓
          Final Answer
```

So:

> **The encoder finds the relevant information; the LLM uses that information to generate the answer.**

* * *

## 7\. Encoder Model and Autoregressive LLM Are Separate

The lecture emphasizes that these two models should not be confused.

```
                 RAG Pipeline

             ┌──────────────────┐
Question ───→ │ Encoder Model   │
             └────────┬─────────┘
                      ↓
                   Vector
                      ↓
             ┌──────────────────┐
             │ Vector Data     │
             │ Store           │
             └────────┬─────────┘
                      ↓
              Relevant TEXT
                      ↓
Question ─────────────┤
                      ↓
             ┌──────────────────┐
             │ Autoregressive  │
             │ LLM              │
             └────────┬─────────┘
                      ↓
                   Answer
```

The **encoder is used for retrieval**, while the **autoregressive LLM is used for generation**.

* * *

## 8\. Why Vectors Are So Useful for RAG

Vectors allow RAG to search based on **meaning rather than exact wording**.

For example:

| User's wording | Database wording | Can semantic search connect them? |
| --- | --- | --- |
| "Ticket prices to Heathrow" | "Ticket prices to London" | Yes |
| Different wording | Similar meaning | Yes |
| Exact same words | Exact same meaning | Yes |

This makes vector search much more flexible than simple keyword matching.

* * *

## 9\. RAG Is Essentially a Retrieval Trick

At its basic level, RAG does this:

```
Question
   ↓
"What does this question mean?"
   ↓
Find database content with similar meaning
   ↓
Retrieve that content
   ↓
Give it to the LLM as context
   ↓
Generate answer
```

The LLM itself doesn't need to contain every piece of knowledge in its parameters.

Instead, RAG **retrieves useful external information and places it into the prompt**.

* * *

## 10\. The Lecture's Important Qualification: RAG Is a "Hack"

The lecturer makes an interesting point: **RAG is somewhat of a hack**.

Why?

Because the system is essentially doing this:

```
User Question
      ↓
Turn it into a vector
      ↓
Guess which stored information
might be useful
      ↓
Retrieve that information
      ↓
Put it into the prompt
```

The system is trying to **empirically determine what context is likely to help the LLM**.

It isn't a perfect, theoretically guaranteed process.

Instead, RAG involves a lot of:

-   experimentation
-   trial and error
-   empirical improvements
-   techniques for selecting better context

The lecturer describes RAG as having a **"zoo of hacks"**—many techniques layered together to improve retrieval quality.

* * *

## 11\. RAG Is Highly Empirical

"Empirical" here essentially means:

> **Try things, measure what works, and improve the system.**

A RAG system may need experimentation around:

```
How documents are split
        ↓
How they are embedded
        ↓
How similarity is measured
        ↓
How many results are retrieved
        ↓
Which context is passed to the LLM
        ↓
How the final prompt is constructed
```

The lecture notes that later work will explore these techniques in practice.

* * *

## 12\. Tools Mentioned for Building RAG

The lecture previews the practical implementation using:

-   **LangChain** — for constructing the RAG pipeline
-   **Chroma** — as the vector store/data store

The planned workflow is:

```
Knowledge Base
      ↓
Create embeddings
      ↓
Store vectors
      ↓
Chroma Vector Store
      ↓
Retrieve relevant information
      ↓
Put retrieved context into prompt
      ↓
LLM
```

* * *

## 13\. What You Should Understand at This Point

The goal at this stage isn't necessarily to know every implementation detail.

The core intuition is:

> **A user's question is converted into a vector, and that vector is used to find stored information with similar meaning. The retrieved natural-language information is then given to an LLM as context so it can produce a better answer.**

* * *

# Key Takeaways

1.  **RAG = Retrieval-Augmented Generation.**
2.  The user's question is converted into a **vector/embedding**.
3.  Knowledge-base content is also converted into vectors beforehand.
4.  Text and its vector are stored together in a **vector data store**.
5.  The vector store performs a **semantic/fuzzy lookup**, not merely an exact keyword search.
6.  Similar meanings tend to produce **similar/nearby vector representations**.
7.  The **encoder model** is used to find relevant information.
8.  The **autoregressive LLM** generates the final response.
9.  **Vectors are not sent to the final LLM**; the retrieved **natural-language text** is.
10.  RAG is highly **empirical** and involves substantial experimentation and trial-and-error.
11.  **LangChain** and **Chroma** are introduced as tools for building the practical RAG pipeline.

## One-line memory aid

**RAG turns the question into a vector → finds similar information in a vector store → retrieves the text → gives that context to the LLM → generates the answer.**