# Week 3, Day 2 — Hugging Face Pipelines

## 1\. What We Already Know

At this point in the course, you can:

-   Work confidently with frontier models.
-   Build multimodal AI systems.
-   Work with tools.
-   Understand the Hugging Face Hub.
-   Understand Hugging Face's open-source libraries.
-   Use Google Colab.

The focus now shifts to actually writing and running **Hugging Face code**.

* * *

# 2\. Two Levels of Hugging Face Code

Hugging Face's libraries can be thought of as having **two levels**:

```
Hugging Face Libraries
│
├── High-level API
│   └── Pipelines
│
└── Low-level API
    ├── Tokenizers
    └── Models
```

## High-Level: Pipelines

**Pipelines** provide a simple interface for common AI inference tasks.

They are designed to:

-   Get something working quickly.
-   Hide unnecessary implementation details.
-   Handle common tasks with very little code.

## Low-Level: Tokenizers and Models

The lower-level API gives much more control.

This is where you can work directly with:

-   Tokens
-   Individual model calls
-   The deep neural network
-   Model internals

It is more powerful but has a steeper learning curve.

The course will spend more time with this lower-level code later, particularly in **Week 7**.

* * *

# 3\. Pipelines vs Tokenizers & Models

|     | Pipelines | Tokenizers & Models |
| --- | --- | --- |
| Level | High | Low |
| Complexity | Simple | More complex |
| Purpose | Common tasks | Detailed model control |
| Speed of development | Fast | Slower |
| Control | Less | Much more |
| Best for | Standard inference tasks | Customization, experimentation, training |

### Mental model

> **Pipelines = quick and simple**

> **Tokenizers + Models = raw power and control**

* * *

# 4\. What Is a Pipeline?

A pipeline is a **high-level API for common inference tasks**.

Inference means using an already-trained model to perform a task.

For example:

```
Input
  ↓
Pre-trained model
  ↓
Result
```

Instead of manually writing all the code required to load and execute the model, Hugging Face provides a pipeline that handles much of this for you.

* * *

# 5\. Common Pipeline Tasks

The lesson introduces several common inference tasks.

## Sentiment Analysis

Determines the sentiment of text.

For example:

```
"This movie was fantastic!"
        ↓
     Positive
```

It can be used for things such as classifying movie reviews as positive or negative.

* * *

## Classification

Takes an input and assigns it to a category.

For example:

```
Input
 ↓
Classifier
 ↓
Person / Place / Other
```

* * *

## Named Entity Recognition (NER)

Identifies and tags entities within text.

For example, it can identify:

-   People
-   Places

Conceptually:

```
"Amit lives in Kolkata."

Amit      → Person
Kolkata   → Place
```

* * *

## Question Answering

Given:

-   A question
-   Some context

the model attempts to produce the answer.

```
Question + Context
        ↓
       Model
        ↓
      Answer
```

This is a core generative-AI task.

* * *

## Summarization

Takes longer text and produces a shorter version containing the important information.

This was already introduced earlier in the course.

* * *

## Translation

Converts text from one language to another.

* * *

# 6\. Pipeline as an Inference Interface

These tasks have a common pattern:

```
Already-trained model
        ↓
     Pipeline
        ↓
     Input
        ↓
     Output
```

The model has already been trained, so the goal is simply to **run it to achieve a particular objective**.

Pipelines make these common use cases much easier.

* * *

# 7\. Basic Pipeline Concept

Creating a pipeline is essentially a two-step process.

### Step 1 — Create the pipeline

You specify the task you want to perform.

Conceptually:

Python

Run

```
pipeline = create_pipeline(task)
```

This creates a pipeline object.

Importantly:

> **Creating the pipeline does not mean you are performing the task yet.**

You are creating something that can perform the task.

### Step 2 — Call the pipeline

You then provide input:

Python

Run

```
result = pipeline(input)
```

The pipeline processes the input and returns the result.

You can call the same pipeline repeatedly with different inputs.

```
Create pipeline
      ↓
   pipeline
   ↙  ↓  ↘
input1 input2 input3
   ↓    ↓    ↓
result result result
```

This is the basic idea behind the **Pipelines API**.

* * *

# 8\. Google Colab

The exercises for this lesson are performed in **Google Colab**.

The course repository contains the relevant notebook:

```
Week 3
└── Day 2
    └── Hugging Face Pipelines
```

The workflow is:

1.  Open the Week 3 folder.
2.  Open Day 2.
3.  Select the Hugging Face Pipelines notebook.
4.  Open it in Google Colab.
5.  Run the examples.

* * *

# 9\. Google Colab: Warnings vs Errors

Data-science environments can produce a lot of output.

You may encounter:

-   Informational messages
-   Warnings
-   Deprecation warnings
-   Other diagnostic messages

Not every warning means something is broken.

### Deprecation warning

A deprecation warning generally means:

> Something currently exists but may be removed or changed in a future version.

The advice from the lesson is to develop an instinct for distinguishing:

```
Warning / informational message
          ≠
Actual error
```

You should pay attention to warnings, but don't automatically stop working because one appears.

If something subsequently fails, earlier warnings may provide clues about what happened.

* * *

# 10\. Important Google Colab GPU Problem

A particularly important issue can occur when using Google Colab.

You may encounter an error similar to:

```
RuntimeError:
CUDA is required but not available
```

The error can be misleading.

You might assume:

> "I need to install CUDA or other packages."

According to the lesson, that is often **not the actual problem**.

* * *

# 11\. What May Actually Have Happened?

Google Colab can move your session from a **GPU machine to a CPU machine**.

Conceptually:

```
You start with:

Google Colab
     ↓
GPU machine
     ↓
Everything works


Later:

Google Colab
     ↓
CPU machine
     ↓
CUDA unavailable
     ↓
Model fails
```

The session may have been moved to a different machine without you realizing it.

The result is that the GPU and related environment you were using are no longer available.

* * *

# 12\. What to Do When This Happens

Do **not** immediately start installing lots of packages.

Instead, reset the Colab runtime.

The procedure given in the lesson is:

### Step 1

Go to:

**Runtime → Disconnect and delete runtime**

### Step 2

Go to:

**Edit → Clear all outputs**

This gives you a clean starting point.

### Step 3

Return to the **top of the notebook**.

### Step 4

Run the notebook from the beginning.

This includes running the `pip install` cells again.

The goal is to ensure that the notebook is running on a fresh GPU environment with everything installed correctly.

* * *

# 13\. Important Troubleshooting Principle

When working with Google Colab:

```
CUDA error
     ↓
Don't immediately install things
     ↓
Check whether you still have a GPU
     ↓
If Colab moved you to CPU:
     ↓
Reset the runtime
     ↓
Run notebook from the beginning
```

The lesson specifically warns against getting stuck in a cycle of unnecessary installations.

* * *

# Key Takeaways

-   Hugging Face code has a **high-level** and **low-level** interface.
-   **Pipelines** are the high-level API.
-   **Tokenizers and Models** provide lower-level control.
-   Pipelines are designed for common, pre-configured inference tasks.
-   Common tasks include:
    
    -   Sentiment analysis
    -   Classification
    -   Named entity recognition
    -   Question answering
    -   Summarization
    -   Translation
-   The basic pipeline workflow is:
    
    1.  Create the pipeline.
    2.  Call the pipeline with input.
    3.  Receive the result.
-   A pipeline can be reused with multiple inputs.
-   Google Colab is used for the exercises.
-   Data-science notebooks can produce many warnings that are not necessarily errors.
-   A particularly important Colab problem is losing access to the GPU.
-   A **CUDA unavailable** error may mean Colab has moved your session from a GPU machine to a CPU machine.
-   In that situation, reset the runtime rather than blindly installing additional packages.
-   The recommended reset procedure is:
    
    -   **Runtime → Disconnect and delete runtime**
    -   **Edit → Clear all outputs**
    -   Run the notebook again from the top.