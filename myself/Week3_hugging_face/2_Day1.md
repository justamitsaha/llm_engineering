# Hugging Face — Open-Source Libraries

## 1\. The Two Sides of Hugging Face

Hugging Face has **two distinct but related parts**:

```
Hugging Face
│
├── 1. Hugging Face Hub
│   ├── Models
│   ├── Datasets
│   └── Spaces
│
└── 2. Hugging Face Libraries
    └── Open-source Python libraries
```

The previous lesson focused on the **Hugging Face Hub**, the online platform containing repositories of models, datasets, and Spaces.

This lesson focuses on the **open-source software libraries** provided by Hugging Face.

* * *

# 2\. Hugging Face Libraries

Hugging Face provides open-source Python libraries that allow you to work directly with open-source Transformer models.

These libraries work with deep-learning frameworks such as:

-   **PyTorch** — the most commonly used in the course
-   TensorFlow
-   JAX

Hugging Face has implemented Transformers using these frameworks so that you can:

1.  Import Python code.
2.  Download model weights.
3.  Run the model yourself.
4.  Inspect and modify the underlying code.

You can run these models in environments such as:

-   JupyterLab
-   Cursor
-   Google Colab

* * *

# 3\. Llama vs Hugging Face Libraries

A key distinction in the lesson is between running a model through **Llama** and running an open-source model directly through **Hugging Face libraries**.

## Llama

Llama, as used in the course, is described as a **software product/application**.

The model is packaged into an efficient format and executed locally using optimized C++ code.

Conceptually:

```
Packaged model
      ↓
Llama application
      ↓
Local execution
      ↓
OpenAI-compatible endpoint
```

You interact with it as an application rather than directly manipulating the model's implementation.

* * *

## Hugging Face Libraries

With Hugging Face, you get access to the **actual Python code** used to work with the model.

Conceptually:

```
Python code
    ↓
Hugging Face libraries
    ↓
Model weights
    ↓
Your local environment
    ↓
Run / modify / train the model
```

This gives you much more control.

You can:

-   Step through the code.
-   Modify the code.
-   Experiment with tokens.
-   Change parts of the neural network.
-   Fine-tune models.
-   Train models.

The key distinction is:

> **Llama provides a packaged software experience, whereas Hugging Face libraries provide access to the underlying model code and tooling.**

* * *

# 4\. Why Hugging Face Libraries Matter

The libraries are a major part of Hugging Face's contribution to open-source AI.

They make it possible to run a wide range of open-source models directly from Python.

The lesson notes that while some early models were available through the ecosystem, today **almost any open-source model you might want to work with can be run using Hugging Face libraries**.

* * *

# 5\. The Six Hugging Face Libraries

The course introduces six libraries that will be used throughout the remainder of the course:

| Library | Main Purpose |
| --- | --- |
| **Hugging Face Hub** | Connect Python code to the Hugging Face Hub |
| **Datasets** | Download and manipulate datasets |
| **Transformers** | Run, use, and train Transformer models |
| **PEFT** | Parameter-efficient fine-tuning |
| **TRL** | Reinforcement learning for Transformers |
| **Accelerate** | Distribute models across multiple GPUs |

* * *

# 6\. Hugging Face Hub Library

This is potentially confusing because the **website/platform is also called the Hugging Face Hub**.

The Python **Hugging Face Hub library** provides the connection between your code and the online Hugging Face Hub.

For example, your Python code can:

-   Log in to Hugging Face.
-   Request a particular model.
-   Download the model.
-   Download datasets.

Conceptually:

```
Your Python code
       ↓
Hugging Face Hub library
       ↓
Hugging Face Hub
       ↓
Model / Dataset
```

### Important distinction

**Hugging Face Hub**

→ The online platform containing models, datasets, and Spaces.

**Hugging Face Hub Python library**

→ The library that lets your Python code communicate with that platform.

* * *

# 7\. Datasets Library

The **Datasets** library works with datasets obtained through the Hugging Face Hub.

Once a dataset is downloaded, it becomes accessible through the Datasets library.

The library provides efficient ways to:

-   Load datasets.
-   Represent datasets as objects.
-   Manipulate datasets.
-   Work with very large amounts of data efficiently.

The course emphasizes its ability to handle **large datasets effectively**.

* * *

# 8\. Transformers Library

This is the **central Hugging Face library**.

It is the library that allows you to work directly with Transformer models.

With Transformers, you can:

-   Select a particular model.
-   Obtain the code needed to run it.
-   Run the model.
-   Train the model.
-   Work directly with the model implementation.

The lesson describes Transformers as the **iconic library at the heart of Hugging Face**.

Conceptually:

```
Hugging Face Transformers
        ↓
Open-source Transformer model
        ↓
Run / Train / Modify
```

* * *

# 9\. PEFT — Parameter-Efficient Fine-Tuning

**PEFT** stands for:

> **Parameter-Efficient Fine-Tuning**

This is one of the more advanced libraries introduced in the course.

The problem it addresses is that a large model may contain billions of parameters.

Instead of modifying all of those parameters during fine-tuning, PEFT provides ways to train models more efficiently.

The course will use PEFT significantly in **Week 7**.

* * *

## LoRA

PEFT also uses techniques such as **LoRA**.

LoRA is a technique for making fine-tuning more efficient rather than directly modifying every parameter in a large model.

The important idea for now is:

```
Traditional fine-tuning
→ Modify a huge number of model parameters

PEFT / LoRA
→ Fine-tune in a more parameter-efficient way
```

The detailed mechanics are covered later in the course.

* * *

# 10\. TRL — Transformers Reinforcement Learning

**TRL** stands for:

> **Transformers Reinforcement Learning**

It is another advanced Hugging Face library.

Its purpose is related to **training Transformer models using reinforcement-learning techniques**.

The course identifies this as one of the more advanced areas that may be explored later.

* * *

# 11\. Accelerate

**Accelerate** is a library designed to make it easier to distribute models across **multiple GPUs**.

Conceptually:

```
Large model
    ↓
Accelerate
    ↓
GPU 1 + GPU 2 + GPU 3 + ...
```

This becomes useful when:

-   Models are too large for a single GPU.
-   You want distributed training.
-   You want to take advantage of multiple GPUs.

The course describes this as part of the more advanced topics involving **distributed training**.

* * *

# 12\. The Overall Hugging Face Ecosystem

The complete picture is:

```
                    HUGGING FACE
                         │
          ┌──────────────┴──────────────┐
          │                             │
     HUGGING FACE HUB              LIBRARIES
          │                             │
    ┌─────┼─────┐          ┌───────────┼────────────┐
    │     │     │          │           │            │
 Models Datasets Spaces    Hub      Datasets   Transformers
                                      │
                                  ┌───┴───────────────┐
                                  │                   │
                                PEFT                 TRL
                                  │
                              Accelerate
```

The important distinction is:

### Hugging Face Hub

The **online platform**:

-   Models
-   Datasets
-   Spaces

### Hugging Face Libraries

The **software you import into Python** to work with those resources:

-   Hub
-   Datasets
-   Transformers
-   PEFT
-   TRL
-   Accelerate

* * *

# Key Takeaways

-   Hugging Face is more than an online model repository.
-   It has both a **platform (Hub)** and a collection of **open-source software libraries**.
-   The libraries allow you to work directly with open-source Transformer models using Python.
-   **PyTorch** is the main deep-learning framework used in this course.
-   Using Hugging Face libraries gives you access to the model code rather than treating the model purely as a packaged application.
-   This makes it possible to **inspect, modify, train, and fine-tune models**.
-   **Transformers** is the central Hugging Face library.
-   **Hugging Face Hub** connects your Python code to models and datasets hosted on the Hub.
-   **Datasets** provides efficient dataset loading and manipulation.
-   **PEFT** enables parameter-efficient fine-tuning, including techniques such as LoRA.
-   **TRL** provides reinforcement-learning tooling for Transformers.
-   **Accelerate** helps distribute models across multiple GPUs.
-   These libraries form a major part of the tooling used in the open-source AI ecosystem.