# Chinchilla Scaling Law

## 1\. What Is the Chinchilla Scaling Law?

The **Chinchilla scaling law** is an idea published by Google about how to scale large language models.

The name comes from Google's **Chinchilla** model, which followed another model called **Gopher**.

The central question was:

> **How should we increase the size of a model to get better performance?**

For example, suppose you have:

```
8 billion parameters
```

and want to build:

```
16 billion parameters
```

Simply doubling the number of parameters does not automatically give you twice the performance.

The Chinchilla research examined how the amount of **training data** needs to scale alongside the number of parameters.

* * *

# 2\. Parameters and Training Data Must Scale Together

The core idea is:

> **If you double the number of parameters, you need roughly twice as much training data to take advantage of them.**

Conceptually:

```
Model A
8B parameters
+
X training tokens

        ↓ Double both

Model B
16B parameters
+
2X training tokens
```

The relationship is between:

-   **Number of model parameters**
-   **Number of training tokens**

* * *

# 3\. Why Training Data Matters

Imagine training an 8-billion-parameter model.

Halfway through your available training data, you notice that the model is no longer improving significantly.

You still have a large amount of unused training data.

The Chinchilla intuition would be:

```
Model capacity is too small
        ↓
More parameters are needed
        ↓
Increase model size
        ↓
Use proportionally more training data
```

The additional parameters provide more capacity for the model to learn from additional training data.

* * *

# 4\. Scaling in Both Directions

The relationship works in both directions.

### Increasing Parameters

If you decide:

> "I want to double the number of parameters."

Then, according to the scaling-law intuition, you should also approximately double the amount of training data.

```
Parameters ×2
      +
Training data ×2
```

### Increasing Training Data

Conversely, if you have substantially more training data available and want to make use of it, you may need a larger model.

```
Training data ×2
      ↓
More model capacity required
      ↓
Parameters ×2
```

* * *

# 5\. Simple Mental Model

A useful way to remember the idea is:

```
More parameters
      ↕
More training data
```

You don't want one to grow dramatically without the other.

The original Chinchilla work therefore provided a useful rule of thumb for thinking about **how model size and training data should scale together**.

* * *

# 6\. Why the Chinchilla Scaling Law Is Less Emphasized Today

The lesson explains that Chinchilla is still useful, but it is discussed less than it was previously.

There are two major reasons.

* * *

## Reason 1 — Better Ways to Pack Information into Models

Researchers have developed increasingly effective ways to get more capability out of fewer parameters.

These include improvements in:

-   Model architectures
-   Training techniques
-   Model compression
-   Pruning
-   Other techniques for making parameters more effective

As a result, **parameter count alone is becoming a less reliable measure of capability**.

For example:

```
Older model
More parameters
        ↓
Potentially less efficient

Newer model
Fewer parameters
        ↓
Better architecture/training
        ↓
More capability than expected
```

* * *

# 7\. Smaller Models Can Be Surprisingly Capable

The course has already encountered examples illustrating this.

### Llama 3.2

A relatively small model can still be surprisingly capable.

### Gemma 3

The course experimented with a model containing only around:

> **270 million parameters**

The model isn't particularly strong compared with modern large models, but the instructor points out that it is still remarkably capable considering its extremely small size.

This demonstrates why simply comparing parameter counts can be misleading.

* * *

# 8\. Reasoning Has Changed the Equation

The second major reason Chinchilla is less prominent is the increasing importance of **inference-time techniques**.

Historically, much of the focus was:

```
Better model
    ↓
Train a bigger model
    ↓
Use more training data
```

Today, performance can also be improved by doing more work **at inference time**.

For example:

```
Training
   +
Inference-time techniques
   ↓
Better performance
```

* * *

# 9\. Reasoning at Inference Time

One important example is **reasoning**.

A model can potentially achieve better results by spending more computation working through a problem before producing its final answer.

Conceptually:

```
Prompt
  ↓
More reasoning
  ↓
Better solution
```

Therefore, simply making the model larger isn't the only way to increase capability.

* * *

# 10\. RAG as an Inference-Time Technique

Another example is **RAG (Retrieval-Augmented Generation)**.

Instead of trying to put every piece of information into the model during training, you can retrieve relevant information when the model is being used.

```
User question
     ↓
Retrieve relevant information
     ↓
Add information to context
     ↓
LLM
     ↓
Answer
```

This allows you to improve the model's ability to answer questions using external information without retraining the underlying model.

* * *

# 11\. Training Time vs Inference Time

This creates an important distinction:

### Training Time

What happens when the model is being created:

```
Training data
     +
Parameters
     ↓
Trained model
```

### Inference Time

What happens when you actually use the model:

```
Trained model
     +
Prompt
     +
Tools / RAG / reasoning
     ↓
Answer
```

Modern LLM engineering increasingly focuses on **both**.

* * *

# 12\. The Updated Mental Model

The older way of thinking was approximately:

```
More capability
      ↓
Bigger model
      ↓
More parameters
      ↓
More training data
```

The modern picture is broader:

```
                 Better performance
                       │
          ┌────────────┴────────────┐
          │                         │
     Training time            Inference time
          │                         │
   More parameters             Reasoning
   More training data          RAG
   Better architecture         Tools
   Better training             Other techniques
```

This is why the Chinchilla scaling law remains useful, but is no longer the entire story.

* * *

# 13\. The Important Qualification

The lesson emphasizes that the Chinchilla scaling law should be treated as a **useful theoretical rule of thumb**, rather than an absolute law that determines modern model performance.

The simple relationship:

```
Parameters ×2
Training data ×2
```

is most useful when comparing models that otherwise have:

-   The same architecture
-   The same training techniques

In other words:

> **Given the same architecture and training approach, doubling the parameters would require roughly twice as much training data to make effective use of the additional capacity.**

* * *

# Key Takeaways

-   The **Chinchilla scaling law** was published by Google.
-   It was named after Google's **Chinchilla** model.
-   It examines the relationship between:
    
    -   **Model parameters**
    -   **Training data**
-   The basic rule of thumb is:

```
Parameters ×2
      ↓
Training data ×2
```

-   Increasing model size without appropriately increasing training data may prevent you from getting the full benefit of the additional parameters.
-   Conversely, substantially more training data may require additional model capacity to use it effectively.
-   Chinchilla remains a useful way to understand the relationship between model size and training data.
-   It is less emphasized today because modern techniques can extract more capability from fewer parameters.
-   Better:
    
    -   Architectures
    -   Training methods
    -   Compression
    -   Pruning  
        can make smaller models surprisingly capable.
-   **Inference-time techniques** have also become increasingly important.
-   Examples include:
    
    -   More reasoning
    -   RAG
    -   Tools
-   Therefore, improving an LLM isn't only about making the model bigger or training it on more data.
-   The modern picture is:

```
Model capability
      =
Training-time improvements
      +
Inference-time improvements
```

### One-line memory aid

> **Chinchilla: bigger models need proportionally more training data—but modern LLM performance increasingly depends on what happens at inference time too.**