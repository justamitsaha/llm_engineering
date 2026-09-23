# Advanced RAG Techniques: Query Expansion, Reranking, Hierarchical RAG, Graph RAG, and Agentic RAG

## 1. The Main Idea: More RAG Techniques, More Experiments

The lecture continues the list of **10 RAG techniques**.

The important mindset remains:

> RAG techniques are not magic solutions. They are different experiments for solving different retrieval problems.

The workflow is:

```text
Identify a RAG problem
        ↓
Choose a possible technique
        ↓
Experiment
        ↓
Run evals
        ↓
Measure improvement
        ↓
Keep / reject the technique
```

Different datasets and business problems can benefit from different techniques.

---

# 2. Technique #6 — Query Expansion

Normally, we might ask an LLM to rewrite a user's question into **one retrieval query**.

**Query expansion** takes this further.

Instead of generating one query, ask the model to generate **multiple alternative queries**.

### Example

User asks:

> "What are the benefits of product X?"

The system might generate:

```text
Query 1 → Benefits of product X
Query 2 → Advantages of product X
Query 3 → Product X use cases
```

Then perform retrieval for all of them.

```text
             User question
                   ↓
                  LLM
                   ↓
        ┌──────────┼──────────┐
        ↓          ↓          ↓
     Query 1    Query 2    Query 3
        ↓          ↓          ↓
      Search     Search     Search
        └──────────┼──────────┘
                   ↓
          Combined chunks
```

### Why?

A single query may fail to capture all the ways the relevant information is expressed.

Multiple queries increase the chances of finding useful context.

---

# 3. Technique #7 — Reranking

Query expansion can produce a **large number of retrieved chunks**.

If:

* `k` = number of chunks retrieved per query
* `n` = number of queries

then potentially you can end up with roughly:

```text
k × n chunks
```

Sending all of them to the final LLM can **pollute the context**.

### Reranking solves this problem.

Take all retrieved chunks and ask another model to order them:

```text
Most relevant
      ↓
Chunk A
Chunk B
Chunk C
Chunk D
      ↓
Least relevant
```

An LLM can be instructed:

> You are not answering the question. Your job is only to rank these chunks from most relevant to least relevant.

Then the system can keep only the most relevant chunks.

```text
Query expansion
      ↓
Many retrieved chunks
      ↓
Reranking
      ↓
Best chunks
      ↓
Final LLM
      ↓
Answer
```

### Main benefit

**Reranking helps prevent irrelevant retrieved context from overwhelming the final LLM.**

---

# 4. Technique #8 — Hierarchical RAG

A major weakness of normal RAG is that it can struggle with questions that require information from **many documents**.

For example:

> "How many employees have a salary below $X?"

This potentially requires examining information about many employees.

A normal vector search may only retrieve a few chunks rather than information covering the entire knowledge base.

### Hierarchical RAG

The idea is to create summaries at different levels.

For example:

```text
Knowledge Base
│
├── Employee Summary
│   ├── Employee documents
│   └── Employee details
│
├── Product Summary
│   ├── Product documents
│   └── Product details
│
└── Contract Summary
    ├── Contract documents
    └── Contract details
```

Then retrieval happens in stages:

```text
User question
      ↓
Search summarized / coarse information
      ↓
Identify relevant area
      ↓
Drill down
      ↓
Search detailed chunks
      ↓
Final answer
```

### Why is this useful?

A summary document might contain information that would otherwise be distributed across hundreds of documents.

For example:

```text
Employee Summary

A → salary $...
B → salary $...
C → salary $...
D → salary $...
...
```

A question involving a count across employees may then become answerable.

---

# 5. The Limitation of Hierarchical RAG

Hierarchical RAG is still a **hack**.

The problem is that you must decide how to organize the hierarchy.

Suppose you build summaries around:

```text
Employees
Products
Contracts
```

But later someone asks:

> "How many documents start with the letter A?"

Your hierarchy wasn't designed around alphabetical information.

So the same **whack-a-mole problem** appears:

```text
Fix one type of question
        ↓
Another question appears
        ↓
Current hierarchy doesn't help
        ↓
Create another structure
```

Therefore, hierarchical RAG is useful but still fundamentally experimental.

---

# 6. Technique #9 — Graph RAG

Documents in a knowledge base are often **related to one another**.

For example:

```text
Employee A
    ↓ manages
Employee B
    ↓ works on
Project X
    ↓ belongs to
Company Y
```

Instead of treating every document/chunk as independent, **Graph RAG represents relationships between them**.

### Basic idea

```text
Chunk A ───────→ Chunk B
  │                 │
  ↓                 ↓
Chunk C ───────→ Chunk D
```

These relationships can be stored in metadata.

For example, a chunk could contain metadata saying:

```text
Related chunks:
- same document
- employee's manager
- associated project
- related product
```

When one chunk is retrieved, the system can also retrieve its related chunks.

---

# 7. Graph Database Approach

A more sophisticated version uses a **graph database**.

The lecture mentions databases such as **Neo4j**, where information is represented as:

```text
Nodes + Edges
```

For example:

```text
       manages
Employee A ─────────→ Employee B
    │                     │
    │ works on            │ works on
    ↓                     ↓
Project X ───────────── Project Y
```

A vector search can find the initial relevant chunk.

Then the graph can provide nearby related information:

```text
Vector search
     ↓
Relevant chunk
     ↓
1-hop / 2-hop neighbors
     ↓
Additional context
```

### When is Graph RAG useful?

It is particularly useful when the data contains **strong relationships between entities**.

However, the instructor argues that a full graph database may be unnecessary in many cases.

Often, relationships can simply be represented using **metadata**.

---

# 8. Technique #10 — Agentic RAG

The final technique is **Agentic RAG**.

Traditional RAG follows a relatively fixed pipeline:

```text
User question
      ↓
Vector search
      ↓
Retrieve context
      ↓
LLM
      ↓
Answer
```

Agentic RAG changes the philosophy.

Instead of hard-coding the retrieval sequence, give an **LLM agent access to multiple tools**.

For example:

```text
                    ┌── Vector search
                    │
User question → Agent ├── SQL database
                    │
                    ├── File search
                    │
                    └── External API
```

The agent decides:

* Which tool to use
* What query to make
* Whether another query is necessary
* Which retrieved information matters
* When it has enough information to answer

---

# 9. Agentic RAG Can Combine Earlier Techniques

An important insight is that Agentic RAG isn't necessarily a completely new retrieval mechanism.

It can perform many of the previous techniques **dynamically**.

For example:

```text
Agent
  ↓
Query rewriting
  ↓
Query expansion
  ↓
Vector search
  ↓
Reranking
  ↓
Another search
  ↓
SQL query
  ↓
Final context
```

The difference is:

**The LLM decides which steps to take and in what order.**

Traditional RAG:

```text
Developer defines workflow
        ↓
Step 1 → Step 2 → Step 3 → Step 4
```

Agentic RAG:

```text
Developer gives tools
        ↓
       Agent
        ↓
Agent decides:
"What should I do next?"
        ↓
Tool call
        ↓
Evaluate result
        ↓
Another tool call?
        ↓
Final answer
```

It can even operate in a loop until it decides it has enough information.

---

# 10. Agentic RAG: Power vs Predictability

Agentic RAG can solve problems that a rigid RAG pipeline might struggle with.

### Potential advantage

```text
Question
   ↓
Agent investigates
   ↓
Search
   ↓
Not enough information
   ↓
Search again
   ↓
Try another tool
   ↓
Combine results
   ↓
Answer
```

This flexibility can produce better answers for complicated questions.

### But there is a trade-off

More autonomy means **less predictability**.

The same agent might behave differently on different runs.

```text
Run 1 → Search A → Search B → Answer
Run 2 → Search A → Search C → Search B → Answer
Run 3 → Search D → Answer
```

This creates challenges around:

* Repeatability
* Robustness
* Reliability
* Evaluation

So agentic systems are powerful but require careful evaluation.

---

# 11. Is RAG Dead?

The lecture addresses a popular claim:

> **"RAG is dead."**

Two common arguments are discussed.

---

## Argument 1 — Context Windows Are Now Huge

One argument says:

> LLM context windows are so large that we can simply put the entire knowledge base into the context.

Then the Transformer's attention mechanism can determine what matters.

The lecture rejects this as a general replacement for RAG.

### Why?

Knowledge bases can become **far larger than practical context windows**.

For example:

```text
Huge knowledge base
        ↓
Put everything into LLM?
        ↓
Extremely expensive / inefficient
```

Instead:

```text
Huge knowledge base
        ↓
Retrieval
        ↓
Discard irrelevant information
        ↓
Send only useful context
```

Even with very large context windows, reducing irrelevant information can remain valuable.

---

# 12. What About Agentic RAG?

The second argument is:

> Traditional RAG is outdated because agents can decide how to retrieve information themselves.

This is a more interesting argument.

Agentic RAG can dynamically perform:

* Query rewriting
* Query expansion
* Retrieval
* Reranking
* SQL queries
* File searches
* API calls
* Multiple retrieval attempts

But the instructor's conclusion is that this is still fundamentally **retrieval-augmented generation**.

The implementation has changed:

```text
Traditional RAG
Developer-controlled retrieval
        ↓
Context
        ↓
LLM
```

versus:

```text
Agentic RAG
LLM-controlled retrieval
        ↓
Context
        ↓
LLM answer
```

The retrieval mechanism may be more dynamic, but the fundamental idea remains:

> **Retrieve relevant information and augment the LLM's generation with it.**

---

# 13. The Evolution of RAG

The techniques from the lecture can be viewed as an evolution:

```text
Basic RAG
   ↓
Chunking
   ↓
Encoder experimentation
   ↓
Prompt engineering
   ↓
Document preprocessing
   ↓
Query rewriting
   ↓
Query expansion
   ↓
Reranking
   ↓
Hierarchical RAG
   ↓
Graph RAG
   ↓
Agentic RAG
```

But this is **not a progression where every later technique replaces the previous one**.

Instead, each technique addresses different problems.

---

# 14. Overall Comparison

| Technique            | Main Problem It Addresses                      |
| -------------------- | ---------------------------------------------- |
| **Query expansion**  | One query may miss relevant information        |
| **Reranking**        | Too many retrieved chunks / irrelevant context |
| **Hierarchical RAG** | Questions spanning many documents              |
| **Graph RAG**        | Relationships between documents/entities       |
| **Agentic RAG**      | Need for flexible, multi-step retrieval        |

And the key philosophy remains:

```text
Technique ≠ Guaranteed solution

Technique
    ↓
Experiment
    ↓
Evaluate
    ↓
Measure
    ↓
Decide whether it helps
```

# Key Takeaways

1. **Query expansion** generates multiple retrieval queries instead of relying on one.
2. **Reranking** reorders retrieved chunks by relevance and can remove low-value context before the final LLM call.
3. **Hierarchical RAG** uses summaries at different levels to handle questions that span many documents.
4. **Graph RAG** exploits relationships between chunks/entities rather than treating documents as isolated.
5. **Agentic RAG** lets an LLM decide which retrieval tools to use and in what order.
6. Agentic RAG can combine many earlier techniques dynamically.
7. **More autonomy means more flexibility but less predictability**, making evaluation especially important.
8. Large context windows do **not automatically make retrieval unnecessary**, particularly when knowledge bases are extremely large.
9. Agentic RAG does not necessarily mean RAG has disappeared—it can be understood as **RAG where the retrieval process is controlled dynamically by an agent**.
10. The overarching lesson is still **experiment + evals** rather than assuming one technique is universally best.

## One-line memory aid

**Query expansion finds more, reranking keeps the best, hierarchical RAG handles broad knowledge, Graph RAG follows relationships, and Agentic RAG lets the LLM decide how to retrieve—but all are still experiments within RAG.**
