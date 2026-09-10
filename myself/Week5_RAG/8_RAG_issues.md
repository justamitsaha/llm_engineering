# RAG with Conversation History: Strengths, Rough Edges, and Evaluation

## 1\. The Starting Problem: Simple RAG Breaks on Follow-up Questions

The lecture begins by revisiting the **simple five-line RAG question-answering implementation**.

It works well for a standalone question:

> “Who is Avery?”

But problems appear when the user asks a follow-up:

> “How much is her salary?”

The system incorrectly answers:

> “Samantha Green's current salary is 70,000.”

### Why?

There are **two separate problems**.

1.  **Conversation history isn't being passed to the LLM**
    
    -   The LLM doesn't know who “her” refers to.
    -   It has no awareness that the previous question was about Avery.
2.  **The RAG lookup only uses the latest question**
    
    -   The retriever sees something like “How much is her salary?”
    -   It doesn't know that “her” means Avery.
    -   It therefore finds some unrelated chunk containing the word **salary**.

So:

```
User: Who is Avery?
          ↓
      RAG lookup
          ↓
       Good answer

User: How much is her salary?
          ↓
   ❌ No conversation history
          ↓
"salary" → unrelated salary chunk
          ↓
   Wrong person / red herring
```

* * *

# 2\. The Two Fixes

The more complete LangChain implementation introduced two fixes.

### Fix #1 — Maintain conversation history

The conversation history is properly constructed and passed into LangChain/LLM.

Now the LLM can understand:

```
User: Who is Avery?
Assistant: Avery Lancaster ...

User: How much is her salary?
              ↑
       "her" = Avery
```

### Fix #2 — Use conversation context for retrieval

Instead of performing the vector lookup using only the latest sentence, the system considers the **combined conversation text**.

Conceptually:

```
Conversation history
        +
Current question
        ↓
Context retrieval
        ↓
Relevant document chunks
        ↓
LLM
        ↓
Answer
```

This makes follow-up questions much more effective.

* * *

# 3\. The Improved Gradio Interface

The new Gradio application provides two useful areas:

| Area | Purpose |
| --- | --- |
| **Conversation** | Shows the ongoing chat |
| **Retrieved Context** | Shows the chunks retrieved by RAG |

This is particularly useful for **debugging RAG** because you can see not only the answer, but also _why_ the system produced it.

The retrieved context displays:

-   The actual document chunks given to the LLM
-   Metadata indicating where those chunks came from

The lecture distinguishes these visually:

-   **White text** → information actually sent to the LLM
-   **Orange text** → metadata/source information for the developer, not necessarily sent to the LLM

* * *

# 4\. Example: Avery's Salary

The improved system is tested again:

### Conversation

```
User: Who is Avery?

User: What is her salary?
```

This time the system correctly understands that **“her” refers to Avery**.

It retrieves multiple relevant chunks, including the chunk containing Avery's salary.

The lecture notes that **K = 5**, so five chunks are retrieved:

```
Vector store
     ↓
Retrieve top 5 chunks
     ↓
Relevant context
     ↓
LLM + conversation history
     ↓
Avery Lancaster's salary
```

This demonstrates **RAG with conversation history** working correctly.

* * *

# 5\. A More Difficult Retrieval Example: Maxine's Award

The instructor then tests a harder question.

The employee database contains information about **Maxine Thompson**, including the fact that she received the prestigious **IIoT award**.

The question is:

> “Who won the prestigious IIoT award?”

The RAG system successfully retrieves the relevant information.

So the system demonstrates an important RAG capability:

> **It can retrieve information buried inside a larger knowledge base even when the question doesn't simply reproduce the exact wording of the document.**

* * *

# 6\. But Chunking Creates a Subtle Problem

Although the system finds the correct information, the answer isn't perfect.

The retrieved chunk says **“Maxine”**, but doesn't contain her full name, **“Maxine Thompson.”**

Why?

Because of the way the original document was divided into chunks.

Imagine the HR document:

```
Maxine Thompson
        ↓
[Large HR document]
        ↓
     ...
        ↓
She received the prestigious IIoT award.
```

The chunk containing the award may look roughly like:

```
... she received the prestigious IIoT award ...
```

while the full name was located earlier:

```
Maxine Thompson
```

That name may belong to a **different chunk**.

Therefore:

```
Question
   ↓
Retriever finds award chunk
   ↓
✓ Correct award information
   ↓
✗ Full employee name missing
```

### Key lesson

A chunk can contain enough information to **answer the question partially**, while still missing crucial context from elsewhere in the document.

This is one of the **rough edges of RAG and document chunking**.

* * *

# 7\. Another Problem: Conversation History Can Become a Liability

The instructor then demonstrates an even more interesting failure.

Start a conversation:

```
User: Who is Avery?

User: What did Avery do before?
```

The system correctly retrieves information about Avery.

Then the user changes topic:

```
User: Who won the prestigious IIoT award?
```

Surprisingly, the system fails to retrieve the correct Maxine information.

### Why?

The retrieval strategy was changed so that it considers the **entire conversation**.

That works beautifully for follow-up questions:

```
Who is Avery?
        +
What is her salary?
```

But it becomes problematic when the conversation changes topics:

```
Who is Avery?
        +
What did Avery do?
        +
Who won the IIoT award?
```

The retriever now sees lots of information about **Avery** alongside the new question about the award.

Consequently, vector similarity may favor older Avery-related chunks.

* * *

# 8\. The Fundamental Trade-Off

This produces an important RAG design tension:

| Strategy | Strength | Weakness |
| --- | --- | --- |
| **Current question only** | Good when topic changes | Poor at resolving follow-up references like “her” |
| **Entire conversation** | Good at understanding follow-ups | Can pollute retrieval when the topic changes |
| **Better retrieval strategy** | Potentially handles both | Requires additional techniques |

In other words:

```
More history
     ↓
Better conversational understanding
     ↓
BUT
     ↓
More irrelevant information
     ↓
Potentially worse retrieval
```

This is why simply adding more conversation history isn't automatically the perfect solution.

* * *

# 9\. RAG Is Experimental and “Hacky”

The instructor emphasizes a major philosophical lesson:

> **RAG can be hit and miss.**

It can work extremely well:

-   It retrieves deeply buried information.
-   It can answer questions over a large knowledge base.
-   It can make a model appear to have expertise over an entire collection of documents.

But it can also fail in surprising ways:

-   Retrieve the wrong person's information
-   Miss important context
-   Produce incomplete answers
-   Get confused when the topic changes
-   Retrieve irrelevant chunks

The instructor describes RAG as essentially a **“hack” or “conjuring trick.”**

The LLM doesn't actually know the entire knowledge base.

Instead:

```
Large knowledge base
        ↓
Semantic vector search
        ↓
A few relevant chunks
        ↓
Put chunks into prompt
        ↓
LLM generates answer
```

The result _looks_ like the LLM has expertise over the whole knowledge base.

But internally, it only has access to the **small set of retrieved chunks for that particular question**.

* * *

# 10\. RAG Improvement Is Like “Whack-a-Mole”

The lecture gives a useful analogy:

> Improving RAG is a bit like **whack-a-mole**.

You find a problem:

```
Problem A
   ↓
Fix A
   ↓
Problem B appears
   ↓
Fix B
   ↓
Problem C appears
```

And sometimes:

```
Fix problem A
      ↓
Creates problem B
```

That's exactly what happened with conversation history.

### Example

**Before the change:**

```
Follow-up question
       ↓
No history
       ↓
"her" is ambiguous
       ↓
Wrong retrieval
```

**After adding history:**

```
Follow-up question
       ↓
History included
       ↓
"her" understood correctly
       ↓
Better retrieval
```

But then:

```
Topic changes
       ↓
Old history still included
       ↓
Old topic influences retrieval
       ↓
New retrieval failure
```

So RAG engineering involves continuously discovering and addressing these interactions.

* * *

# 11\. The Scientific Way Forward: Evaluation

The instructor introduces the next major topic:

## **RAG evaluation / evals**

Rather than relying only on intuition and manually looking at a few examples, the goal is to create **metrics and evaluations** that measure how well the RAG system performs.

This allows systematic experimentation with things such as:

-   Different **embedding/encoder models**
-   Different **chunking strategies**
-   Different retrieval approaches
-   Other RAG techniques

The basic loop becomes:

```
Change RAG technique
        ↓
Run evaluation
        ↓
Measure performance
        ↓
Compare against previous version
        ↓
Keep / reject the change
```

This is much more scientific than simply saying:

> “This version feels better.”

* * *

# 12\. The Assignment: Explore the Rough Edges

Before the next lecture, the instructor asks students to run the application themselves.

The workflow is essentially:

```
1. Run ingest.py
       ↓
2. Populate vector store
       ↓
3. Run app.py
       ↓
4. Ask many questions
       ↓
5. Inspect retrieved chunks
       ↓
6. Identify strengths and failures
```

Students should deliberately look for both:

### Excellent RAG examples

Cases where the system:

-   Finds information buried deep in documents
-   Demonstrates strong semantic understanding
-   Retrieves the right chunks
-   Gives an expert-quality answer

### Failure cases

Cases where:

-   The answer is clearly present in the documents
-   Yet RAG fails to retrieve it
-   The wrong chunk is retrieved
-   Important context is missing
-   The answer is incomplete
-   Conversation history causes confusion

These failures become a **list of problems to solve later**.

* * *

# 13\. What You Have Built So Far

The lecture ends with a recap of how much has been accomplished.

The student can now build a system that:

```
Documents
    ↓
Chunking
    ↓
Vectorization / embeddings
    ↓
Vector store
    ↓
Semantic retrieval
    ↓
Relevant context
    ↓
LLM
    ↓
Question answering
    ↓
Gradio UI
```

In practical terms, the student can write:

-   `ingest.py` → process and store documents
-   `app.py` → run the RAG application
-   Supporting code → connect embeddings, vector store, retriever, and LLM

The result is an application that **appears to be an expert over an entire knowledge base**.

* * *

# 14\. The “Dark Secret” of RAG

The most important conceptual point is this:

**The LLM does NOT have the entire knowledge base in its context.**

Instead:

```
                Entire Knowledge Base
                         │
                         ▼
                 Vector similarity
                         │
                         ▼
                A few relevant chunks
                         │
                         ▼
                  Prompt to LLM
                         │
                         ▼
                     Answer
```

Therefore, the apparent expertise comes from repeatedly performing:

> **retrieve → inject context → generate**

This is what makes RAG powerful while also creating its weaknesses.

* * *

# 15\. Key Concepts Introduced

| Concept | Meaning |
| --- | --- |
| **Conversation history** | Previous turns provided to the LLM so references such as “her” can be understood |
| **History-aware retrieval** | Using conversation information when searching the vector store |
| **Chunking problem** | Important information can be separated across chunks |
| **Topic drift** | Old conversation content can interfere when the user changes subjects |
| **Retrieved context** | The document chunks selected for the current answer |
| **RAG rough edges** | Situations where retrieval produces incomplete, irrelevant, or wrong context |
| **Evaluation / evals** | Systematic measurement of RAG quality |
| **Semantic search** | Finding information by meaning rather than exact keyword matching |

* * *

# Key Takeaways

1.  **Simple RAG can fail on follow-up questions** because the retriever and LLM may not know the conversation history.
2.  **Conversation history helps resolve references** such as “her,” “him,” “that company,” etc.
3.  **Using the entire conversation for retrieval has a downside:** old topics can contaminate retrieval when the user changes subjects.
4.  **Chunking can separate important pieces of context.** A retrieved chunk may contain the answer but omit the person's name or other crucial information.
5.  **RAG doesn't give the LLM the whole knowledge base.** It retrieves a small number of relevant chunks and puts them into the prompt.
6.  **RAG is powerful but imperfect.** Retrieval quality is highly dependent on how the system chunks, embeds, searches, and constructs context.
7.  **RAG improvement is iterative.** Fixing one problem can sometimes introduce another.
8.  **Retrieved context should be inspected.** Looking at the actual chunks is one of the best ways to understand why a RAG system succeeded or failed.
9.  **Evaluation/evals are essential.** They allow you to measure whether changes to encoders, chunking, and retrieval actually improve the system.
10.  **The next stage is disciplined experimentation:** instead of merely building a RAG system, systematically measure and improve it.

## One-line memory aid

**RAG is a retrieval trick: conversation history can make follow-ups smarter, but can also pollute retrieval—so inspect the chunks, find the rough edges, and use evaluations to improve the system scientifically.**