# RAG Evaluations: Bringing Scientific Rigor to RAG

## 1\. The Central Question: Is Your RAG System Actually Working?

After successfully building a RAG expert Q&A system, the next challenge is:

> **How do we know whether it actually works well?**

It is not enough for the system to produce _some_ answers.

We need to measure:

1.  **Retrieval performance** — Does RAG retrieve the right context?
2.  **Question-answering performance** — Does the LLM produce a good answer from that context?

The second is ultimately closest to the **business objective**, but both matter.

```
                 RAG System
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
   Retrieval Quality      Answer Quality
   "Did we find the       "Did we answer
    right context?"        correctly?"
```

Evaluations, or **evals**, provide a way to measure these systematically.

* * *

# 2\. Why RAG Is So Attractive

Before discussing its weaknesses, the instructor highlights several major strengths.

### A. Very fast to build

A useful RAG system can be created in **minutes**.

You can ingest documents, create embeddings, populate a vector store, and quickly have a question-answering system over the knowledge base.

### B. Scalable

The same approach can handle much larger datasets.

Even if ingesting 100× more information takes considerably longer, the process can run in the background, potentially overnight.

Once the vector store is populated, querying it can remain very fast.

```
Small knowledge base
        ↓
      RAG
        ↓
Expert Q&A

Massive knowledge base
        ↓
      RAG
        ↓
Expert Q&A
```

### C. Much easier than fine-tuning

Fine-tuning, which will be covered later, can be:

-   Difficult
-   Time-consuming
-   Expensive

RAG is comparatively quick and straightforward.

### D. Saves context-window space

Instead of giving the LLM the entire knowledge base, RAG selects only the information that is likely to matter.

```
Entire Knowledge Base
        ↓
   Vector Search
        ↓
Relevant information
        ↓
      LLM
```

This means the context window isn't filled with unnecessary information.

### E. Reduces irrelevant context

Giving an LLM huge amounts of irrelevant information can make it harder to focus on the answer.

RAG attempts to **focus the context window on the information relevant to the current question**.

* * *

# 3\. The Fundamental Weakness: RAG Is a “Hack”

Despite all these benefits, the instructor emphasizes that RAG is essentially a **performance optimization / shortcut**.

Transformers are already designed to determine which parts of an input deserve attention.

But when there is a huge amount of information, asking the Transformer to process everything can be inefficient.

RAG instead tries to reduce the amount of information first:

```
Huge amount of information
          ↓
Encoder + vector similarity
          ↓
"These chunks are probably relevant"
          ↓
Discard the rest
          ↓
LLM processes selected context
```

The critical word is **“probably.”**

Vector similarity does not guarantee that the retrieved chunks are the correct chunks.

Therefore, RAG is fundamentally a heuristic.

* * *

# 4\. Why RAG Is Empirical

The instructor repeatedly emphasizes that RAG is **empirical**.

In this context, empirical essentially means:

> **Try things, measure what happens, and keep what works.**

There isn't always a universal answer to questions such as:

-   Which encoder should I use?
-   How large should chunks be?
-   How much overlap should I use?
-   Which retrieval technique should I use?
-   Which RAG architecture is best?

You can have a good intuition, but you don't truly know until you **test it**.

```
Idea
 ↓
Implement
 ↓
Evaluate
 ↓
Better?
 ↙   ↘
Yes    No
 ↓      ↓
Keep   Try something else
```

This experimental mindset is central to RAG engineering.

* * *

# 5\. The “Whack-a-Mole” Problem

One of the lecture's recurring analogies is **whack-a-mole**.

You fix one RAG problem, and another problem appears somewhere else.

For example:

```
Fix retrieval problem A
          ↓
Problem B appears
          ↓
Fix problem B
          ↓
Problem C appears
          ↓
Fix problem C
          ↓
Problem A may change again
```

This happens because different RAG components interact.

A change that improves one type of question can make another type of question worse.

This makes RAG development difficult to reason about intuitively.

* * *

# 6\. Why “Obvious” RAG Improvements Can Fail

Suppose you ask:

> “Who won the prestigious award?”

You know the answer is clearly somewhere in the documents.

But the RAG system responds:

> “I don't know.”

You investigate and discover:

-   The relevant information exists.
-   Retrieval isn't surfacing the right chunk.
-   Your chunking strategy may be responsible.
-   Another parameter or technique may need to change.

You modify the system.

Now the award question works—but perhaps another question gets worse.

This demonstrates why **intuition alone isn't enough**.

* * *

# 7\. Chunking Is a Perfect Example

The lecture uses **chunking** to demonstrate why RAG requires experimentation.

The basic idea is:

```
Large document
      ↓
Split into chunks
      ↓
Chunk 1
Chunk 2
Chunk 3
...
      ↓
Embed each chunk
      ↓
Store vectors
```

The assumption is that a question is more likely to match the relevant **part of a document** than the entire document.

But chunk size creates a trade-off.

* * *

# 8\. Problem with Chunks That Are Too Large

Imagine Maxine's entire HR document contains lots of information:

```
Maxine's HR Record
├── Employment history
├── Performance
├── Awards
├── Salary
├── Promotions
└── Other information
```

If the entire document is one chunk, the question:

> “Who won the IIoT award?”

has to match against **all of Maxine's information**.

The question may not be sufficiently similar to the complete document.

Therefore, retrieval may become less precise.

### General principle

**Large chunks contain more context, but can be less focused for retrieval.**

* * *

# 9\. Problem with Chunks That Are Too Small

Now make the chunks extremely small.

You might get:

```
Chunk:
"She received the prestigious IIoT award..."
```

This is highly relevant to the question.

But perhaps the chunk doesn't contain:

> “Maxine Thompson”

because her name appeared in an earlier chunk.

Now you have the opposite problem:

```
Small chunk
   ↓
✓ Contains answer
   ↓
✗ Missing crucial context
   ↓
Incomplete answer
```

This was exactly the issue demonstrated in the previous lecture.

### General principle

**Small chunks improve retrieval specificity but can lose important surrounding context.**

* * *

# 10\. Chunk Size Is a Trade-Off

Therefore, there is no universally correct chunk size.

You may need:

-   Smaller chunks
-   Larger chunks
-   More overlap
-   Less overlap

depending on the documents and questions.

```
             Chunking
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
     Small    Medium    Large
       │        │        │
   Focused    Balance   More context
       │        │        │
   Less       ?        Less precise
   context
```

The optimal choice must be **evaluated experimentally**.

* * *

# 11\. Evals Turn RAG From Alchemy Into Science

This is the most important idea of the lecture.

Without evaluations:

```
Try something
     ↓
Looks better?
     ↓
Maybe keep it
```

With evaluations:

```
Try something
     ↓
Measure performance
     ↓
Compare against baseline
     ↓
Improvement?
     ↓
Make evidence-based decision
```

This converts RAG development from something resembling **alchemy** into a more scientific engineering process.

* * *

# 12\. The North-Star Metric

The instructor describes an evaluation metric as a **North Star**.

You establish a measurable target:

> “This is my metric, and I will keep iterating until this number improves.”

For example:

```
Baseline RAG
     ↓
Evaluation score = X
     ↓
Change chunking
     ↓
Evaluation score = Y
     ↓
Compare X vs Y
```

If `Y > X`, the change may be beneficial.

If `Y < X`, the change probably isn't helping according to that metric.

This provides an objective basis for decisions.

* * *

# 13\. Evals Are Important Beyond RAG

The instructor makes a broader point:

**Working with LLMs is inherently experimental.**

You frequently have to experiment with:

-   Prompts
-   Models
-   RAG strategies
-   Chunking
-   Encoders
-   Retrieval techniques
-   Parameters

This can feel unsatisfying if you're relying only on subjective judgment.

Evals provide the missing structure:

```
Experiment
    ↓
Measure
    ↓
Compare
    ↓
Iterate
    ↓
Measure again
```

Thus, evaluations are not merely a RAG technique—they are a **general methodology for LLM engineering**.

* * *

# 14\. What Evals Will Enable

Once evaluations exist, the course can systematically experiment with:

### Different encoders

```
Encoder A → Evaluation score
Encoder B → Evaluation score
Encoder C → Evaluation score
```

### Different chunking strategies

```
500-token chunks → score
1000-token chunks → score
1500-token chunks → score
```

### Different RAG approaches

Each approach can be tested against the same evaluation criteria.

The goal isn't:

> “Which approach sounds best?”

It becomes:

> **“Which approach actually performs best according to our chosen evaluation?”**

* * *

# 15\. The Complete Mental Model

The lecture's overall philosophy can be summarized as:

```
              RAG System
                  │
       ┌──────────┴──────────┐
       ▼                     ▼
  Retrieval              Generation
       │                     │
"Did we find           "Did the LLM
 the right chunks?"     answer well?"
       │                     │
       └──────────┬──────────┘
                  ▼
               EVALS
                  │
                  ▼
        Quantitative measurement
                  │
                  ▼
             Experiment
                  │
                  ▼
              Improve
                  │
                  └───────→ Repeat
```

The central engineering loop is:

**Build → Measure → Experiment → Compare → Improve → Repeat**

* * *

# Key Takeaways

1.  **Building RAG is easy; knowing whether it works well is much harder.**
2.  RAG performance should be evaluated at both the **retrieval level** and the **final answer level**.
3.  RAG is attractive because it is **fast to build, scalable, cheaper than fine-tuning, and efficient with context length**.
4.  RAG is fundamentally a **heuristic/hack**: vector similarity only estimates which information is probably relevant.
5.  Because RAG is heuristic, it is inherently **empirical and experimental**.
6.  There is no universally correct choice for **encoder, chunk size, overlap, or retrieval technique**. You need to test them.
7.  **Chunking involves trade-offs**:
    
    -   Large chunks → more context but potentially poorer retrieval precision.
    -   Small chunks → better focus but potentially missing crucial context.
8.  RAG development can feel like **“whack-a-mole”** because fixing one problem can introduce another.
9.  **Evals provide the scientific discipline that RAG lacks on its own.**
10.  A good evaluation gives you a **North Star metric** for deciding whether each change actually improves the system.
11.  The most important workflow is:

```
Build → Evaluate → Experiment → Measure → Improve
```

12.  **Evals are a broader LLM-engineering skill**, not merely a RAG-specific technique.

## One-line memory aid

**RAG is an experimental retrieval hack, and evals turn that trial-and-error process into measurable, scientific iteration.**