# Advanced RAG Techniques: Chunking, Encoders, Pre-processing, and Query Rewriting

## 1. RAG Techniques Are Experimental, Not Magic Solutions

As RAG became popular, a large ecosystem of techniques and terminology developed around improving RAG systems.

The instructor’s central warning is:

> These techniques should be viewed as **experimental techniques**, not as guaranteed architectures or magic solutions.

RAG development is often a process of:

```text
RAG system
    ↓
Find a problem
    ↓
Form a hypothesis
    ↓
Try a technique
    ↓
Run evals
    ↓
Measure the result
    ↓
Keep / reject the change
```

Different datasets and business problems can respond differently to the same technique.

### Key principle

**Do not ask: "Which RAG technique should I use?"**

Instead:

**Set metrics → create evals → run experiments → measure → iterate.**

There is no shortcut around experimentation.

---

# 2. Technique #1 — Experiment with Chunking

One of the first areas to experiment with is **how documents are chunked**.

Different chunking strategies and parameters can produce very different retrieval results.

For example, you can experiment with:

* Different text splitters
* Different chunk sizes
* Different overlap
* Different ways of identifying chunk boundaries
* Different semantic structures

LangChain contains many different splitters/chunkers that can be experimented with.

### Best practice

Don't randomly change chunking just because you can.

Instead:

```text
Problem observed
      ↓
Hypothesis:
"Maybe chunking is causing this"
      ↓
Change chunking strategy
      ↓
Run evals
      ↓
Compare results
```

So chunking experimentation should ideally be **hypothesis-driven**.

---

# 3. Technique #2 — Select and Experiment with Encoders

The **embedding/encoder model** is another important part of advanced RAG experimentation.

The goal is to determine which encoder works best for:

* Your particular dataset
* Your questions
* Your business objective

The instructor emphasizes that the correct encoder cannot simply be declared in advance.

Instead:

```text
Encoder A
   ↓
Run test set
   ↓
Measure

Encoder B
   ↓
Run test set
   ↓
Measure

Choose based on evidence
```

The same encoder may perform differently on different business problems.

---

# 4. Handling Images in RAG

Knowledge bases are not always text-only. They may contain **images as well as text**.

There are multiple possible approaches.

### Approach A — Multimodal encoder

Use an encoder that can map both:

* Text
* Images

into a common vector space.

The transcript mentions that Sentence Transformers/Hugging Face provide models designed for this.

However, the instructor describes these approaches as **hit and miss**, meaning they should be tested rather than assumed to work.

### Approach B — Convert the image into text

Another strategy is:

```text
Image
  ↓
Generate useful textual description/caption
  ↓
Vectorize the text
  ↓
Store/query the vector
```

The idea is to create a textual representation that is likely to match questions users will ask.

The instructor personally tends to prefer this approach in some situations, but explicitly emphasizes that it depends on the use case and should be tested.

---

# 5. Handling PDFs and Other File Formats

A common question is:

**"How do we vectorize a PDF or Word document?"**

The transcript recommends **not treating the raw PDF binary object as something to directly encode**.

Instead:

```text
PDF / Word / Other format
          ↓
Parse / convert using software
          ↓
Text / Markdown
          ↓
Optional LLM processing
          ↓
Chunks
          ↓
Embeddings
          ↓
Vector database
```

### Important point: Use ordinary software for format conversion

The instructor strongly argues that converting file formats is a **solved software problem**.

For example:

* Python libraries can parse PDFs.
* Documents can be converted to text or Markdown.
* There is generally no need to use an LLM merely to convert a PDF into Markdown.

The principle is:

> Use conventional software where conventional software already solves the problem.

Then, if useful, an LLM can process the resulting text to make it better suited for querying.

---

# 6. Technique #3 — Prompt Engineering

The next technique is surprisingly simple:

**Improve the prompt.**

Although prompt engineering may not sound like an "advanced RAG technique," it can have a significant effect.

RAG provides retrieved context, but there can also be **static context** that should appear in every prompt.

For example:

* Current date
* Relevant background information
* Conversation history
* Instructions about how to use retrieved context

A useful conceptual structure is:

```text
Fixed context
     +
Conversation context
     +
Retrieved RAG context
     +
User question
     ↓
    LLM
     ↓
   Answer
```

The instructor emphasizes that spending a few hundred extra tokens on useful context can be worthwhile if it significantly improves the quality of answers.

---

# 7. Technique #4 — Document Pre-processing

This is presented as a more advanced and powerful technique.

Instead of taking documents exactly as they are and immediately putting them into the vector store, first **rewrite or transform the documents into a form that is better for retrieval**.

### Example: Tables

Imagine a knowledge base contains a table of ticket prices:

```text
Origin | Destination | Price
----------------------------
A      | B           | ...
A      | C           | ...
...
```

If you directly embed the table, the resulting vector may not represent the information in a way that is particularly useful for natural-language questions.

Instead:

```text
Original document
       ↓
LLM / preprocessing
       ↓
Retrieval-friendly text
       ↓
Embedding
       ↓
Vector store
```

The LLM can rewrite the information into descriptive text that is more likely to match a user's question.

### Core idea

**Don't just vectorize the document as-is.**

Ask:

> "What representation of this information would make semantic retrieval work better?"

---

# 8. Semantic Chunking

A related technique is **semantic chunking**.

Traditional chunking may split documents according to structural boundaries such as:

```text
paragraph
paragraph
paragraph
```

Semantic chunking instead attempts to divide a document according to **meaning**.

For example, a person's document might contain sections about:

```text
Career history
     ↓
Education
     ↓
Technical skills
     ↓
Awards
```

Or a product document might naturally divide into:

```text
Features
     ↓
Pricing
     ↓
Technical specifications
     ↓
Use cases
```

The goal is for each chunk to represent a meaningful concept.

---

# 9. LLM-Based Document Pre-processing

Semantic chunking can also be performed as part of document preprocessing.

For example:

```text
Document
   ↓
LLM
   ↓
"Break this document into meaningful chunks
that will work well for knowledge-base queries."
   ↓
Meaningful chunks
   ↓
Rewrite chunks if necessary
   ↓
Vectorize
```

The instructor describes this combination of:

* Semantic chunking
* Document rewriting
* Retrieval-oriented preprocessing

as a **professional/pro technique**.

The objective is to make it more likely that a user's question corresponds cleanly to one or a small number of chunks.

---

# 10. Technique #5 — Query Rewriting

There is a flip side to document preprocessing:

**Query rewriting.**

Instead of changing the documents, change the user's question before performing retrieval.

This is useful when the original question isn't ideal for vector search.

### Example: Follow-up questions

Suppose the conversation is:

```text
User: Who is John?
       ↓
System discusses John

User: What is his salary?
```

The second question by itself is ambiguous.

Instead of searching for:

```text
"What is his salary?"
```

an LLM can rewrite it into something more useful for retrieval:

```text
"What is John's salary?"
```

The conversation history can be supplied to the rewriting LLM so it understands what the user is referring to.

---

# 11. Query Rewriting Pipeline

The process can be viewed as:

```text
Conversation history
        +
Current user question
        ↓
       LLM
        ↓
Rewritten retrieval query
        ↓
Vector search
        ↓
Relevant documents
        ↓
RAG LLM
        ↓
Final answer
```

The important distinction is that the LLM used for rewriting is helping **prepare the query for retrieval**.

It is not necessarily the final answer-generating step.

---

# 12. Document Pre-processing vs Query Rewriting

These two techniques form a powerful pair:

| Technique                   | What is changed?         | Goal                                             |
| --------------------------- | ------------------------ | ------------------------------------------------ |
| **Document pre-processing** | Knowledge-base documents | Make stored information easier to retrieve       |
| **Query rewriting**         | User's question          | Make the retrieval query easier to match         |
| **Semantic chunking**       | Document structure       | Create meaningful retrieval units                |
| **Prompt engineering**      | LLM instructions/context | Help the final model use information effectively |

Conceptually:

```text
          DOCUMENT SIDE
               ↓
Document → Pre-process → Semantic chunks → Embeddings
                                              ↓
                                         Vector DB
                                              ↑
                                              │
                                      Query rewriting
                                              ↑
                                              │
User question + history ──────────────────────┘
```

Both sides can be optimized independently.

---

# 13. The Overall RAG Experimentation Mindset

The techniques discussed are not a checklist where every RAG system should implement everything.

Instead:

```text
                 RAG problem
                      ↓
             Identify failure mode
                      ↓
              Form hypothesis
                      ↓
        ┌─────────────┴─────────────┐
        ↓                           ↓
 Change documents              Change queries
        ↓                           ↓
 Chunking / preprocessing      Query rewriting
        ↓                           ↓
 Encoder experiments           Prompt/context
        └─────────────┬─────────────┘
                      ↓
                    Evals
                      ↓
                 Compare results
                      ↓
                  Iterate
```

Different datasets and business use cases will benefit from different combinations.

Therefore, **evaluation is the mechanism that tells you whether a technique actually helped.**

---

# Key Takeaways

1. **Advanced RAG techniques are experiments, not magic solutions.**
2. **Never assume a particular RAG technique will work for your problem.** Test it.
3. **Chunking R&D** can significantly affect retrieval quality.
4. **Encoder selection** should be based on your dataset, test set, and business objective.
5. For **images**, you can experiment with multimodal encoders or convert images into retrieval-friendly text.
6. For **PDFs and documents**, use conventional parsing/software first; don't use an LLM for a problem that ordinary software already solves.
7. **Prompt engineering** remains important even in advanced RAG systems.
8. **Document preprocessing** transforms raw knowledge into a representation that is easier to retrieve.
9. **Semantic chunking** attempts to divide documents according to meaning rather than arbitrary boundaries.
10. **Query rewriting** transforms a user's question into a form better suited for knowledge-base retrieval.
11. **Document preprocessing + query rewriting** optimize both sides of the retrieval process.
12. The central workflow is:

```text
Identify problem
      ↓
Form hypothesis
      ↓
Try RAG technique
      ↓
Run evals
      ↓
Measure
      ↓
Iterate
```

## One-line memory aid

**RAG has no magic technique: optimize chunks, encoders, documents, queries, and prompts experimentally—and let evals tell you what actually works.**
