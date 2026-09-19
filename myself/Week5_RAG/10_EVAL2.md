# RAG Evaluations: Golden Data, Retrieval Metrics, and Answer Quality

## 1. The Three-Step Evaluation Process

The lecture introduces a practical framework for evaluating RAG systems:

```text
┌─────────────────────────────────────────────┐
│          RAG Evaluation Process              │
├─────────────────────────────────────────────┤
│ 1. Build a Golden Dataset                   │
│            ↓                                │
│ 2. Measure Retrieval Effectiveness          │
│            ↓                                │
│ 3. Measure Answer Quality                   │
│            ↓                                │
│      Experiment & Improve                   │
└─────────────────────────────────────────────┘
```

The goal is to bring **scientific rigor** to RAG, which otherwise tends to be highly experimental.

---

# 2. Step 1 — Build a Golden Dataset

The first requirement is a **golden dataset** (or golden test set).

This is a curated collection of:

* Questions
* Expected/reference answers
* Information that should appear in retrieved context

For example:

| Question                            | Expected keywords | Reference answer                                |
| ----------------------------------- | ----------------- | ----------------------------------------------- |
| Who won the prestigious IIoT award? | Maxine, Thompson  | Maxine Thompson won the prestigious IIoT award. |

The golden dataset establishes:

> **What does “good” look like?**

---

# 3. Questions Should Ideally Come From Real Users

You can create test questions yourself by studying your documents.

For example:

> “What questions would I consider important enough that my RAG system should answer them correctly?”

However, an even better source is **real user questions**.

Imagine a production system where employees currently email experts with questions.

You might have:

```text
Real user question
        +
Real expert answer
        ↓
Golden evaluation example
```

This is extremely valuable because it represents the **actual problems users care about**.

### Best source of golden data

The lecture's practical recommendation is:

**Real user questions + real expert answers** are an excellent source of golden test data.

---

# 4. The Golden Dataset Should Be a Living Dataset

The test set shouldn't necessarily be created once and forgotten.

It should evolve.

```text
Initial test set
      ↓
Find a failure
      ↓
Add the failure as a test
      ↓
Improve RAG
      ↓
Find another failure
      ↓
Add another test
      ↓
Repeat
```

This is important because of **overfitting to the test set**.

If you repeatedly optimize your RAG system against only a small fixed collection of questions, it may become very good at those particular questions while performing poorly on new types of questions.

Therefore:

**Continuously expand the evaluation dataset, especially when you discover new failure cases.**

---

# 5. Why Generalization Matters

Suppose your test set contains:

```text
Question 1
Question 2
Question 3
...
Question 100
```

You optimize the system until it performs extremely well on all 100.

That doesn't necessarily mean the system is generally good.

A new user might ask:

```text
Question 101
```

and the system could fail badly.

This is a classic **generalization problem**.

Therefore, a strong evaluation dataset should contain a wide variety of realistic questions.

---

# 6. Step 2 — Evaluate Retrieval

The second major evaluation is:

> **Did RAG retrieve the right context?**

This evaluates the retrieval component independently of the final LLM answer.

```text
Question
   ↓
Retriever
   ↓
Top K chunks
   ↓
❓ Are the relevant chunks here?
```

There are several metrics for this.

The lecture introduces:

* **MRR — Mean Reciprocal Rank**
* **NDCG — Normalized Discounted Cumulative Gain**
* **Recall**
* **Precision**
* **Keyword coverage percentage**

---

# 7. MRR — Mean Reciprocal Rank

**MRR** measures how high the first relevant result appears.

The basic formula is:

```text
MRR = average of 1 / rank of first relevant result
```

So:

| First relevant chunk |      Score |
| -------------------: | ---------: |
|                  1st |          1 |
|                  2nd |  1/2 = 0.5 |
|                  3rd | 1/3 ≈ 0.33 |
|                  4th | 1/4 = 0.25 |
|                  5th | 1/5 = 0.20 |

### Example

Suppose the relevant chunk is always the first retrieved chunk:

```text
Question A → Relevant chunk #1
Question B → Relevant chunk #1
Question C → Relevant chunk #1
```

Then:

**MRR = 1**

That represents perfect first-result ranking according to this metric.

### Memory aid

**MRR asks: “How quickly do I find the first relevant chunk?”**

---

# 8. NDCG — Normalized Discounted Cumulative Gain

NDCG considers **multiple relevant results**, rather than only the first one.

It asks roughly:

> **Are the relevant chunks concentrated near the top of the results?**

For example:

```text
Results:

#1  ✓ Relevant
#2  ✓ Relevant
#3  ✓ Relevant
#4  ✗ Irrelevant
#5  ✗ Irrelevant
```

This is better than:

```text
#1  ✗
#2  ✗
#3  ✗
#4  ✓
#5  ✓
```

because relevant information is surfaced earlier.

The lecture notes that NDCG uses a formula involving logarithms, but the exact formula is less important here than the concept.

### Memory aid

**NDCG asks: “How well are the relevant results distributed toward the top?”**

---

# 9. Recall@K

For RAG, **recall is particularly important**.

Recall@K asks:

> **In what percentage of test cases is the relevant information present somewhere within the top K retrieved chunks?**

For example:

**Recall@3 = 50%**

means:

> In 50% of the test questions, the top 3 retrieved chunks contained relevant context.

```text
Question
   ↓
Top 3 chunks
   ↓
┌───────┬───────┬───────┐
│ Chunk │ Chunk │ Chunk │
│   1   │   2   │   3   │
└───────┴───────┴───────┘
          ↓
   Relevant anywhere?
```

If yes → **hit**.

---

# 10. Keyword Coverage

The lecture adapts the recall idea to a situation where multiple keywords matter.

For example:

### Question

> “Who won the prestigious IIoT award?”

Required keywords might be:

```text
Maxine
Thompson
```

Suppose the retrieved chunks contain:

```text
✓ Maxine
✗ Thompson
```

Then the retrieval is incomplete.

If both appear:

```text
✓ Maxine
✓ Thompson
```

the context has complete keyword coverage.

Therefore, **keyword coverage percentage** measures how many required keywords are successfully surfaced across the evaluation set.

It is essentially a **recall-style metric**.

---

# 11. Precision

Precision looks at retrieval from the opposite direction.

Instead of asking:

> “Did I find the relevant information?”

it asks:

> **“Of everything I retrieved, how much was actually relevant?”**

Suppose RAG always retrieves 5 chunks.

If 3 are relevant:

```text
3 relevant
─────── = 60% precision
5 total
```

So:

**Precision = relevant retrieved chunks / total retrieved chunks**

---

# 12. Recall vs Precision in RAG

The distinction is important:

| Metric        | Main question                              |
| ------------- | ------------------------------------------ |
| **Recall**    | Did I retrieve the relevant information?   |
| **Precision** | How much of what I retrieved was relevant? |

For many RAG applications, the lecture emphasizes **recall / keyword coverage** more strongly.

Why?

Because having some irrelevant information isn't necessarily disastrous.

The bigger problem is:

> **Failing to retrieve the information that actually matters.**

However, precision becomes important when irrelevant chunks are causing problems or distracting the model.

---

# 13. Step 3 — Evaluate the Actual Answers

Retrieval isn't the ultimate goal.

The ultimate goal is:

> **Does the system give the user a good answer?**

You therefore compare:

```text
Reference answer
        vs
RAG-generated answer
```

For example:

### Reference

> Maxine Thompson won the prestigious IIoT award.

### Generated answer

> Maxine won the prestigious IIoT award.

The generated answer contains the correct core information, but it may be considered incomplete because **“Thompson”** is missing.

---

# 14. LLM-as-a-Judge

One approach is simple word matching.

But that is limited because two correct answers may use completely different words.

Instead, the lecture introduces:

## **LLM-as-a-Judge**

Another LLM evaluates the generated answer against the reference answer.

Conceptually:

```text
Reference Answer
       +
Generated Answer
       ↓
   Judge LLM
       ↓
Evaluation score
```

The judge can be instructed to determine whether the generated answer is good relative to the reference.

The lecture recommends using a **stronger LLM as the judge** when possible.

A lightweight model can be sufficient for simple comparisons, but stronger models are generally more capable judges.

---

# 15. Three Dimensions of Answer Quality

The lecture identifies three useful dimensions.

## 1. Accuracy

Is the answer **correct**?

```text
Correct information?
        ↓
      YES ✓
```

## 2. Completeness

Does the answer contain **all the important information**?

For example:

```text
"Maxine won the award."
```

may be accurate but less complete than:

```text
"Maxine Thompson won the prestigious IIoT award."
```

## 3. Relevance

Does the answer contain **only information relevant to the question**?

An answer can be accurate but unnecessarily long or filled with unrelated details.

---

# 16. The Ideal Answer

A high-quality answer should therefore be:

```text
          GOOD ANSWER
               │
      ┌────────┼────────┐
      ▼        ▼        ▼
  Accurate  Complete  Relevant
      │        │        │
      └────────┼────────┘
               ▼
        High-quality answer
```

### Example

Question:

> “Who won the IIoT award?”

Good answer:

> “Maxine Thompson won the prestigious IIoT award.”

It is:

* **Accurate** → correct person
* **Complete** → full name and award
* **Relevant** → directly answers the question

---

# 17. Retrieval Metrics vs Answer Metrics

The lecture highlights an important distinction.

### Retrieval metrics

These are **closer to the RAG system itself** and easier to control.

If retrieval performance drops, you can inspect:

* Which chunks were retrieved?
* What was their ranking?
* What chunking strategy was used?
* Which encoder was used?

You can then make a change and immediately measure its effect.

```text
Change chunking
      ↓
Run retrieval
      ↓
Measure MRR / recall / coverage
      ↓
See immediate effect
```

### Answer metrics

These are **closer to the actual user/business objective**.

But they are harder to debug because many components can contribute to a bad answer.

```text
Bad answer
   ↓
Was retrieval bad?
Was context incomplete?
Was prompt bad?
Did LLM misunderstand?
Was reference inadequate?
```

So answer quality is harder to trace back to one specific cause.

---

# 18. End-User Feedback Is Even Further Downstream

There is an additional evaluation layer:

## **Real user feedback**

For example, a user might indicate:

* Which answer they prefer
* Whether an answer was useful
* Whether the response solved their problem

This is closest to the actual user experience.

The lecture uses ChatGPT-style feedback as an example of collecting information about answer preferences.

Conceptually:

```text
Retrieval quality
       ↓
Answer quality
       ↓
User feedback
```

As you move downward:

**Business relevance increases, but diagnosing the underlying technical cause becomes harder.**

---

# 19. The Evaluation Hierarchy

The complete picture is:

```text
             RAG SYSTEM
                 │
                 ▼
       ┌───────────────────┐
       │ Retrieval Quality │
       │ MRR / Recall etc. │
       └─────────┬─────────┘
                 ↓
       ┌───────────────────┐
       │  Answer Quality   │
       │ Accuracy /        │
       │ Completeness /    │
       │ Relevance         │
       └─────────┬─────────┘
                 ↓
       ┌───────────────────┐
       │  User Feedback    │
       └───────────────────┘
```

Each layer answers a different question.

---

# 20. How to Use Evals for Experimentation

Once you have a golden dataset and metrics, you can systematically change your RAG system.

For example:

```text
Baseline
   ↓
Evaluate
   ↓
Change chunk size
   ↓
Evaluate
   ↓
Change encoder
   ↓
Evaluate
   ↓
Change retrieval strategy
   ↓
Evaluate
```

Now you have evidence for whether a change helped.

Instead of:

> “I think this approach is better.”

you can ask:

> **“What happened to my evaluation metrics?”**

---

# 21. The Most Important Three Things

The instructor emphasizes that memorizing every formula isn't the main goal.

The most important ideas are:

### 1. Curate a test set

Create a **golden dataset** of representative questions and expected answers.

### 2. Measure retrieval

Determine whether RAG is successfully surfacing the information it needs.

Useful concepts include:

* **MRR**
* **NDCG**
* **Recall@K**
* **Keyword coverage**
* **Precision**

### 3. Measure answer quality

Compare generated answers with reference answers using methods such as **LLM-as-a-Judge**.

Evaluate:

* **Accuracy**
* **Completeness**
* **Relevance**

Then use these measurements as the foundation for experimentation.

---

# Key Takeaways

1. **A golden dataset is the foundation of RAG evaluation.**

2. A golden test case should contain a **question, expected/reference answer, and information/keywords expected in the retrieved context**.

3. **Real user questions and expert answers** are an especially valuable source of golden data.

4. The golden dataset should be **continuously expanded**, especially when new failure cases are discovered.

5. **MRR** measures how high the first relevant result appears.

6. **NDCG** measures how well relevant results are positioned toward the top of the retrieved list.

7. **Recall@K** measures how often relevant context appears somewhere within the top K results.

8. **Keyword coverage** is a recall-style measure useful when multiple important pieces of information must be retrieved.

9. **Precision** measures how much of the retrieved context is actually relevant.

10. For RAG, **recall/keyword coverage is often more important than precision**, because missing the necessary information is usually worse than retrieving some extra information.

11. Retrieval quality is technically closer to the RAG mechanism and therefore **easier to iterate on and diagnose**.

12. **Answer quality is more aligned with the real business/user objective**, but is harder to debug.

13. **LLM-as-a-Judge** allows another LLM to evaluate a generated answer against a reference answer.

14. Answer quality can be evaluated using **accuracy, completeness, and relevance**.

15. **User feedback** is even closer to the ultimate objective, but it is further downstream and harder to connect directly to technical changes.

16. The overall evaluation loop is:

```text
Golden Dataset
      ↓
Measure Retrieval
      ↓
Measure Answers
      ↓
Experiment
      ↓
Measure Again
      ↓
Improve RAG
```

## One-line memory aid

**Build a golden dataset, measure whether retrieval finds the right information, measure whether the answer is accurate/complete/relevant, and use those metrics as your North Star for improving RAG.**
