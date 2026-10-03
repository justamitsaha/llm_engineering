# Week 6, Day 5 — Fine-Tuning a Frontier Model

## 1. Where We Are

The course has progressed through:

* Open- and closed-source models
* RAG solutions
* The five-step AI problem-solving strategy
* Data curation and preprocessing
* Traditional ML baselines
* Neural networks
* Evaluation using the **ŷ vs. y chart**

Now the next step is:

**Fine-tuning a frontier model**

The lecture distinguishes this from next week's deeper work with open-source models. With a frontier model, most of the complexity is handled by the provider through an API.

---

# 2. Frontier Model Fine-Tuning vs. Open-Source Fine-Tuning

There are two different experiences being discussed.

### Fine-tuning a frontier model

With a provider such as OpenAI:

* Prepare the training data.
* Upload it.
* Request fine-tuning through the API.
* Monitor training.
* Evaluate the resulting model.

You don't directly work with the internal training machinery.

### Fine-tuning an open-source model

Next week, the course will go deeper into the internals, including concepts such as:

* LoRA
* LoRA matrices
* Target modules
* Model parameters
* Actual training mechanics

### Key distinction

**Frontier model:**

`Your data → API → Provider handles training`

**Open-source model:**

`Your data → Your training process → Direct control over fine-tuning`

---

# 3. What Is LoRA?

The lecture briefly introduces **LoRA** but deliberately postpones the detailed explanation until next week.

LoRA stands for:

**Low-Rank Adaptation**

The basic idea is that instead of modifying the enormous frontier model directly, a relatively small set of additional parameters is trained.

Conceptually:

**Large pretrained model**
+
**Small set of learned parameters**
===================================

**Customized behavior**

The large model remains essentially the underlying foundation, while the smaller learned component nudges its behavior for the specific task.

For today's frontier-model exercise, OpenAI handles this mechanism behind the scenes.

---

# 4. The Three-Stage Fine-Tuning Process

Fine-tuning through an API follows three major stages.

## Stage 1 — Create Training Data

Prepare a dataset specifically for fine-tuning.

The training data is supplied as a:

**JSON file**

Each line contains a separate JSON document/example.

Conceptually:

```text
Training examples
        ↓
JSONL format
        ↓
Upload to provider
```

The training examples contain the inputs and the desired outputs.

---

## Stage 2 — Run the Training

Once the data is uploaded, the fine-tuning job is started.

During training, you can monitor things such as:

* Training progress
* Loss
* Loss for individual batches
* How the loss changes over time

The goal is to see whether the model is learning from the training examples.

---

## Stage 3 — Evaluate and Iterate

After training:

1. Evaluate the model.
2. Examine the results.
3. Adjust the training setup/data if necessary.
4. Run another experiment.
5. Compare the results.

So the practical workflow is:

**Create data → Train → Evaluate → Adjust → Repeat**

This is another example of the course's emphasis on experimentation rather than expecting a single perfect configuration.

---

# 5. Fine-Tuning Gives You a Customized Model

When fine-tuning a frontier model, the result is effectively a **private customized version** of the model for your use.

The underlying frontier model remains enormous, but the fine-tuning process creates a smaller set of learned parameters that modifies its behavior.

At inference time, those learned parameters are applied to the larger underlying model.

Conceptually:

```text
Large pretrained model
        +
Fine-tuned parameters
        ↓
Customized model
```

---

# 6. Types of Fine-Tuning

The lecture introduces several fine-tuning approaches:

1. **SFT — Supervised Fine-Tuning**
2. **DPO — Direct Preference Optimization**
3. **RFT — Reinforcement Fine-Tuning**

There is also a vision-related fine-tuning option, but it isn't relevant to this particular product-price prediction project.

---

# 7. SFT — Supervised Fine-Tuning

### Basic idea

You provide:

**Input → Correct output**

The model learns from examples where the desired answer is already known.

For example:

```text
Product description
        ↓
Expected price
```

If the dataset contains the correct price for each product, those examples can be used as supervised training data.

### SFT is useful for

The lecture gives examples including:

* Classification
* Translation with specific nuances
* Generating content in a particular style
* Correcting recurring model failures

### Core idea

**Known correct answers → Train the model to reproduce the desired behavior**

---

# 8. DPO — Direct Preference Optimization

DPO is based on **preferences** rather than simply providing one correct answer.

Imagine a model produces two possible responses:

```text
Response A 👍
Response B 👎
```

Human feedback tells the system which response is preferred.

The model can then be fine-tuned to produce responses more like the preferred examples.

### DPO is useful for

* Style
* Tone
* Conversational behavior
* Emphasizing desired aspects of responses
* Learning subjective human preferences

The lecture connects this approach to the broader history of **reinforcement learning from human feedback (RLHF)**.

### Core distinction

**SFT:**

`Here is the correct answer.`

**DPO:**

`Here are preferred and less-preferred answers.`

---

# 9. RFT — Reinforcement Fine-Tuning

RFT is presented as a more advanced approach for achieving strong performance within a particular domain.

The important difference is that you don't necessarily have a predefined correct answer for every training example.

Instead, you have a **grader** that evaluates the model's output.

The grader can potentially be another LLM.

This is sometimes described as:

**LLM-as-a-judge**

### Process

```text
Input
  ↓
Model generates answer
  ↓
Grader evaluates answer
  ↓
Score / feedback
  ↓
Fine-tuning
  ↓
Improved model
```

This allows the model to be shaped according to more nuanced objectives.

The lecture gives examples such as evaluating whether an answer satisfies complex requirements.

### Key distinction

**SFT:**
You have the correct answer.

**RFT:**
You have a way to judge whether the answer is good.

---

# 10. SFT vs. DPO vs. RFT

| Method  | What You Provide                      | Main Idea                             |
| ------- | ------------------------------------- | ------------------------------------- |
| **SFT** | Correct examples                      | Learn from labeled input/output pairs |
| **DPO** | Preferred vs. less-preferred examples | Learn human preferences               |
| **RFT** | A grading mechanism                   | Learn from evaluated outputs          |

### Simple mental model

**SFT → Correct answer**

**DPO → Preferred answer**

**RFT → Good/bad score from a grader**

---

# 11. Why SFT Is Right for This Project

The project is predicting product prices.

The dataset already contains the correct answers:

**Product description → Actual price**

Therefore, the training data is **labeled**.

There is no need to build a separate grader because the actual price provides the ground truth.

For example:

```text
Input:
"Product description..."

Correct output:
"$49.99"
```

This makes the project a natural use case for:

**Supervised Fine-Tuning (SFT)**

---

# 12. Why Not RFT?

RFT is useful when there isn't necessarily a single predefined correct answer but you can build a mechanism to determine whether an answer is good.

For the price prediction project, however:

**Actual price = known ground truth**

Therefore, creating an LLM-based grader simply to determine whether the predicted price matches the actual price would add unnecessary complexity.

SFT can directly use the labeled data.

---

# 13. Why SFT Is the Common Starting Point

The lecture describes SFT as the most commonly used approach among these methods.

The reason is straightforward:

If you have:

**Labeled training data**

then you can directly use those examples to teach the model the desired behavior.

DPO and RFT become particularly useful when the objective involves:

* Human preferences
* Nuanced behavior
* Subjective quality
* Complex grading criteria

---

# 14. The Important Concept: Fine-Tuning Does Not Mean Retraining GPT From Scratch

A major point is that you're **not training the enormous GPT model from scratch**.

Instead, the provider handles the underlying infrastructure and trains a relatively small set of parameters that modifies the behavior of the larger pretrained model.

Conceptually:

```text
Huge pretrained model
        ↓
Small trainable adaptation
        ↓
Customized behavior
```

This is why API-based fine-tuning is practical compared with attempting to train a frontier model from scratch.

---

# 15. Overall Fine-Tuning Workflow

For this project, the process is:

```text
Curated Product Dataset
        ↓
Prepare Labeled Training Examples
        ↓
Convert to JSONL
        ↓
Upload to OpenAI
        ↓
Start SFT Training
        ↓
Monitor Loss
        ↓
Evaluate Fine-Tuned Model
        ↓
Compare Against Existing Models
        ↓
Modify / Experiment
        ↓
Repeat
```

---

# 16. Connection to the Earlier AI Strategy

This fits directly into the five-step AI strategy learned earlier.

### Understand

Define the business problem:

**Predict the price of a product.**

### Prepare

* Curate data
* Preprocess descriptions
* Create datasets

### Select

Compare:

* Baselines
* Traditional ML
* Neural networks
* Frontier models

### Apply / Customize

Use:

* Prompting
* RAG
* Agentic AI
* **Fine-tuning**

### Productionize

Eventually:

* Deploy
* Monitor
* Evaluate
* Scale
* Maintain

Fine-tuning is therefore one possible **customization technique**, not the entire AI development process.

---

# 17. The Key Mental Model

The most important concepts from this lecture can be reduced to:

### Fine-tuning

**Take a pretrained model → train it further on task-specific examples → get more specialized behavior.**

### SFT

**Input + known correct output → model learns the desired mapping.**

### DPO

**Preferred output vs. less-preferred output → model learns preferences.**

### RFT

**Model output → grader → feedback → model learns from the score.**

### For this project

Because the actual product price is known:

**Product description + actual price → SFT**

---

# Key Takeaways

1. **Fine-tuning specializes a pretrained frontier model for a particular use case.**
2. With a frontier provider, much of the underlying training complexity is handled through an **API**.
3. The basic workflow is:

   * Create training data
   * Run training
   * Evaluate and iterate
4. Fine-tuning data is supplied in **JSON/JSONL format**.
5. **SFT** uses labeled examples with known correct outputs.
6. **DPO** learns from preferred and less-preferred outputs.
7. **RFT** uses a grader to evaluate generated outputs and provide feedback.
8. For the product-price project, **SFT is appropriate because actual prices provide ground truth**.
9. Fine-tuning does **not** mean training the enormous frontier model from scratch.
10. The provider can train a smaller set of adaptation parameters that modifies the behavior of the larger pretrained model.
11. The next part of the course will go deeper into **LoRA and open-source model fine-tuning**.
12. The overall learning progression is:

**Traditional ML → Neural Networks → Frontier Models → Fine-Tuning → Open-Source Fine-Tuning → RAG + Agentic AI**

### One-line summary

> **Fine-tuning takes a pretrained frontier model and specializes its behavior using task-specific examples; for this project, labeled product descriptions and prices make Supervised Fine-Tuning (SFT) the natural approach.**
