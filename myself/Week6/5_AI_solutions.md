# AI Strategy for Solving Commercial Problems

## 1. The Five-Step Strategy

When building an AI system for a commercial problem, don't start by asking **“What AI architecture should I use?”**

Instead, approach it like a science/project problem:

**Understand → Prepare → Select → Apply → Productionize**

| Step                 | What it means                                                                   |
| -------------------- | ------------------------------------------------------------------------------- |
| **1. Understand**    | Understand the business problem and requirements                                |
| **2. Prepare**       | Prepare data and candidate models                                               |
| **3. Select**        | Experiment and select the model that performs best                              |
| **4. Apply**         | Apply the model using techniques such as prompting, RAG, agents, or fine-tuning |
| **5. Productionize** | Put the solution into production                                                |

---

# 2. Step 1 — Understand the Problem

The most important principle is:

> **Start with the commercial problem, not the AI solution.**

### Gather business requirements

Don't accept a requirement such as:

> “We need an AI agent.”

That's already a **solution**, not a business requirement.

Instead, determine:

* What actual business problem needs to be solved?
* Is AI even the appropriate solution?
* How will success be measured?
* What data is available?

  * How much?
  * What quality?
  * What format?
  * Structured or unstructured?
* What are the non-functional requirements?

  * Scalability
  * Acceptable latency
  * Budget
  * Time to market
  * When the system needs to be live

### Key principle

**Understand the problem → define success → understand the data → understand constraints.**

---

# 3. Step 2 — Prepare

Before jumping into an advanced LLM solution, investigate simpler approaches.

### A. Build a baseline

Consider:

1. Existing solutions
2. Python/non-AI solutions
3. Traditional machine learning
4. Then more sophisticated AI/LLM approaches

The goal is to create a **baseline model**.

A baseline gives you a starting point for comparison.

If a simple solution already solves the problem sufficiently well, there may be no reason to build something much more complicated.

### B. Select candidate LLMs

Use the model-selection approach covered earlier:

* Benchmarks
* Leaderboards
* Arenas
* Other relevant evidence

Use these to create a **candidate set of LLMs** for further experimentation.

### C. Curate the data

Build a high-quality dataset for training.

You can even create **multiple candidate datasets**, train with each, and compare their performance.

**Data quality is part of model preparation.**

---

# 4. Step 3 — Select the Model

Once you have:

* The business problem understood
* Prepared datasets
* Candidate models

you can begin experimentation.

### Process

**Candidates → Experiments → R&D → Prototypes → Training → Validation → Selection**

The objective is to identify the model that performs best **for the specific problem being solved**.

---

# 5. Step 4 — Apply the Model

After selecting a model, there are several ways to improve how it solves the business problem.

The transcript discusses four major techniques:

### 1. Prompting

Improve the instructions given to the model.

One example is **multi-shot prompting**, where multiple examples are provided:

**Question → Answer → Question → Answer → ...**

This can encourage more consistent outputs.

---

### 2. RAG — Retrieval-Augmented Generation

RAG provides the model with relevant information at inference time.

Typical process:

**User question → Retrieve relevant context → Add context to prompt → LLM generates answer**

Retrieval can use:

* Embeddings
* Vector similarity
* Knowledge bases

RAG is useful when you want the model to use **existing knowledge/data**.

---

### 3. Agentic AI

Agentic systems can divide a task into multiple LLM calls.

An **orchestrator LLM** can determine:

* Which other LLM/tool to call
* What order to call them in
* What work needs to be performed

Agents can also use tools to:

* Investigate data
* Perform research
* Scrape the web
* Carry out actions

The transcript describes agentic AI as partly an extension of prompting and RAG.

---

### 4. Fine-Tuning

Fine-tuning takes a model and trains it further using specialized/proprietary data.

The goal is to make the model better at a particular task.

**Pre-trained model → Fine-tuning data → Further training → Specialized model**

---

# 6. Inference Time vs Training Time

An important distinction is introduced.

### Inference-time techniques

These change **how you use the model after it has been trained**.

The transcript places these in this category:

* Prompting
* RAG
* Agentic AI

They improve performance through the way the model is called and the information/workflow provided to it.

### Training-time technique

**Fine-tuning** changes the model itself through additional training.

| Approach    | Main idea                             |
| ----------- | ------------------------------------- |
| Prompting   | Change the instructions               |
| RAG         | Give the model relevant information   |
| Agents      | Use multiple calls/tools/workflows    |
| Fine-tuning | Train the model to behave differently |

### Important point

Training time and inference time are **two different, complementary ways** of improving model performance.

They are not necessarily an either/or choice.

---

# 7. RAG vs Fine-Tuning

The transcript gives an important conceptual distinction.

### RAG

RAG is primarily about giving the model **information that already exists**.

For example:

> “Here is the knowledge from our company database. Use it to answer the question.”

RAG can provide subject-matter expertise from a knowledge base, but the transcript says it generally does not teach the model to generalize beyond that knowledge base.

### Fine-Tuning

Fine-tuning is about teaching the model a **new skill or behavior** that it can generalize to new data.

Conceptually:

**RAG → Give the model knowledge**

**Fine-tuning → Teach the model a skill**

---

# 8. Why Fine-Tuning Fits the Price Prediction Project

The capstone involves estimating the price of products.

The goal isn't simply:

> “Estimate the price of products already present in our dataset.”

Instead, the desired capability is:

> Learn a general skill for estimating product prices and apply it to products the model has never seen before.

That makes fine-tuning a potentially good fit because the objective is **generalization to new data**.

However, the instructor emphasizes that this should still be tested experimentally.

---

# 9. There Is No Universal “Right” Technique

A major lesson is:

> **Don't assume in advance which technique will work best.**

Instead:

1. Try prompting.
2. Try RAG.
3. Try agentic approaches where appropriate.
4. Try fine-tuning.
5. Combine techniques when useful.
6. Use an evaluation metric.
7. Compare performance against the business objective.

In short:

**R&D → Experiment → Measure → Compare → Choose based on results**

There is no shortcut around experimentation.

---

# 10. Overall Workflow

The complete strategy can be visualized as:

**Understand the business problem**
↓
**Define success + constraints**
↓
**Prepare data**
↓
**Build a baseline**
↓
**Select candidate models**
↓
**Run experiments**
↓
**Select the best-performing model**
↓
**Apply prompting / RAG / agents / fine-tuning**
↓
**Evaluate against the business objective**
↓
**Productionize**

---

# Key Takeaways

* **Start with the business problem, not the AI architecture.**
* An “AI agent” is a solution, not a business requirement.
* Always establish a **baseline** before building something sophisticated.
* Data preparation and curation are critical to successful AI development.
* Use experiments and evaluation rather than assumptions to select models and techniques.
* **Prompting, RAG, and agentic AI** primarily operate at inference time.
* **Fine-tuning** operates at training time.
* **RAG provides knowledge; fine-tuning can teach a new skill.**
* Training-time and inference-time improvements can be combined.
* For the Price Is Right project, fine-tuning is being explored because the goal is to **generalize price estimation to previously unseen products**.
* Ultimately, the correct approach is determined through **R&D and measurement against the business objective**, not by choosing a technique in advance.
