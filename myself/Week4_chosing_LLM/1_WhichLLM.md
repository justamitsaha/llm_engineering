# How to Choose the Right LLM

## 1\. The Most Important Question: Which LLM Is Best?

The central question of Week 4 is:

> **Which is the best LLM?**

The answer is:

> **There is no single best LLM.**

"Which model is best?" is an **ill-posed question**.

The useful question is:

> **Which is the right model for the task I am trying to solve?**

A model should therefore be selected based on the **requirements of the particular problem**, rather than simply choosing the model with the highest benchmark score.

Pasted text

* * *

# 2\. The Model Selection Strategy


```
Understand the requirements
          ↓
Look at basic model information
          ↓
Create a candidate set
          ↓
Compare benchmarks
          ↓
Test suitability for the actual task
          ↓
Choose the model
```

The process starts with understanding **what the application actually needs**.

Only then should you start comparing models.

* * *

# 3\. Two Levels of Model Evaluation

Once the requirements are understood, evaluate candidate models in two broad ways:

### 1\. Basic Model Information

Things such as:

-   Open vs closed source
-   Chat vs reasoning vs hybrid
-   Release date
-   Knowledge cutoff
-   Number of parameters
-   Training tokens
-   Context window
-   Cost
-   Build cost
-   Time to market
-   Rate limits
-   Speed
-   Latency
-   License

### 2\. Benchmarks

Look at how the models perform on relevant tasks:

-   Coding
-   Language understanding
-   Reasoning
-   Other task-specific capabilities

Leaderboards and model arenas can help with this comparison.

* * *

# 4\. First Decision: Chat, Reasoning, or Hybrid?

One of the first characteristics to consider is the **type of model**.

### Chat Models

Designed primarily to respond directly and naturally to prompts.

Useful when you want:

-   Fast responses
-   Conversation
-   Creative generation
-   General-purpose interactions

### Reasoning Models

Designed to spend more effort reasoning through difficult problems.

They often perform better on:

-   Reasoning tasks
-   Difficult problems
-   Some benchmarks

However, they have disadvantages.

They tend to be:

-   Slower
-   More expensive in some cases
-   Less suitable for some creative-generation tasks

Therefore:

> **A reasoning model is not automatically better.**

* * *

# 5\. Hybrid Models

Some models can effectively decide whether they need to:

```
User request
     ↓
   Model
   ↙   ↘
Chat   Reason
```

These are **hybrid models**.

They can be useful because the model can choose the appropriate behavior depending on the task.

However, even a hybrid model isn't always the best choice.

For example, if your application specifically needs a fast chat model, a dedicated chat model may be preferable.

* * *

# 6\. Model Release Date

The **release date** is another basic characteristic worth checking.

A newer model may contain more recent training and potentially offer improved capabilities.

But the release date alone isn't enough to determine which model is best.

* * *

# 7\. Knowledge Cutoff

The **knowledge cutoff** tells you how recent the information in the model's training data is.

For example, suppose a model was trained with information up to:

> July 2026

It can already have knowledge of events and information up to that point as part of its learned knowledge.

You don't need to provide that information again.

But information after the cutoff isn't automatically part of the model's knowledge.

* * *

# 8\. Handling Information Beyond the Cutoff

If you need more recent information, you can provide it at **inference time**.

For example:

```
Model knowledge
     +
Recent information
     ↓
Prompt
     ↓
Model
```

You can supply recent information through:

-   Prompts
-   Tools
-   Other inference-time techniques

This allows a model to work with information that wasn't present in its original training data.

* * *

# 9\. Number of Parameters

The number of **parameters** provides an indication of the model's size.

Parameters are the numerical values learned during training.

Generally:

> **More parameters → potentially more capability**

As a rule of thumb, larger models tend to be more capable.

However, this is no longer an absolute rule.

Modern model-development techniques can allow smaller models to achieve surprisingly strong performance.

Therefore:

> **Don't choose a model purely by parameter count.**

* * *

# 10\. Training Tokens

Another useful number is the amount of data used to train the model.

This is often described in terms of **training tokens**.

Conceptually:

```
More training data
        ↓
More information available during training
        ↓
Potentially more knowledge/capability
```

The amount of training data is therefore another useful basic characteristic when comparing models.

* * *

# 11\. Context Window

The **context window** is the model's total capacity for processing the conversation/context.

It isn't simply the current prompt.

It can include:

```
First user message
      +
Assistant response
      +
Next user message
      +
Next response
      +
...
      +
Current request
```

All of this needs to fit within the model's context window.

A larger context window can therefore be important for applications involving:

-   Long conversations
-   Large documents
-   RAG
-   Extensive instructions
-   Large amounts of retrieved information

* * *

# 12\. Model Cards

Much of the basic information about a model can be found in its:

> **Model Card**

Model providers publish model cards containing information about their models.

Therefore, when investigating a model, the model card is one of the first places to look.

Leaderboards can also present much of this information in a format that makes comparison easier.

* * *

# 13\. Cost

Cost is one of the most important practical considerations.

There isn't just one type of cost.

For cloud models:

```
API usage
   ↓
Input/output token costs
```

For locally run models:

```
Model
 ↓
Your hardware
 ↓
Compute cost
```

A locally running model might appear to be "free," but the computation still has a cost.

Your computer consumes:

-   GPU resources
-   Electricity
-   Hardware capacity
-   Time

If you run it on company infrastructure, that infrastructure also has a cost.

Therefore:

> **Local inference isn't truly free; you're simply paying for the compute differently.**

* * *

# 14\. Additional Training Cost

You may also need to consider the cost of **training the model further**.

For example, if you're planning to fine-tune a model, you need to account for:

```
Base model
    +
Additional training
    ↓
Training compute cost
```

This becomes particularly important when building a solution around an open-source model that requires customization.

* * *

# 15\. Build Cost

Another cost is the **cost of building the application itself**.

A model isn't necessarily the entire solution.

You may need to build:

-   Prompts
-   Tools
-   RAG
-   Evaluation systems
-   Infrastructure
-   User interfaces
-   Integration code
-   Fine-tuning pipelines

Therefore, two models with different API prices can have very different **total project costs**.

* * *

# 16\. Time to Market

A closely related factor is:

> **Time to market**

This asks:

> How long will it take to build and deploy the solution?

Suppose you use a very capable frontier model.

You may need relatively little engineering work:

```
Powerful model
      ↓
Less application-level work
      ↓
Faster development
      ↓
Faster time to market
```

A smaller, cheaper model may have lower runtime costs but require significantly more engineering and experimentation.

```
Smaller model
     ↓
More experimentation
     ↓
More engineering
     ↓
Longer development
```

Therefore:

> **The cheapest model to run isn't necessarily the cheapest model overall.**

* * *

# 17\. Rate Limits

Cloud models may impose **rate limits**.

A rate limit restricts how frequently your application can call the model.

For example:

```
Application
   ↓
API
   ↓
Rate limit
   ↓
Maximum requests / usage
```

Higher usage tiers can often provide higher limits, particularly when you're willing to pay more.

However, even premium accounts can have limits.

Therefore, rate limits need to be considered when designing an application that may generate many simultaneous requests.

* * *

# 18\. Speed

Another major consideration is **model speed**.

Different models can have dramatically different response speeds.

Examples mentioned in the lesson include:

-   Gemini Flash-Lite
-   GPT-5 Nano
-   GPT-4.1 Nano

Smaller models are often particularly attractive when low latency is important.

The key point is:

> **Model intelligence isn't the only performance characteristic that matters.**

A slightly less capable model that responds much faster may be the better choice for a real-time application.

* * *

# 19\. Latency

Closely related to speed is **latency**.

One particularly important measurement is:

> **Time to First Token (TTFT)**

This measures how long it takes before the model starts returning output.

This is particularly important for models that perform substantial reasoning before producing an answer.

For a streaming user interface, the experience can look like:

```
User sends request
       ↓
     Wait...
       ↓
First token appears
       ↓
More tokens stream
       ↓
Complete response
```

The time before that first token appears can strongly affect the perceived responsiveness of an application.

* * *

# 20\. Speed vs Latency

These are related but aren't exactly the same thing.

### Latency / Time to First Token

How long before the model **starts responding**.

### Speed

How quickly the model **generates the response once it starts**.

Therefore, evaluate both:

```
Time to first token
        +
Generation speed
        ↓
Overall user experience
```

* * *

# 21\. License

The **license** is easy to overlook but extremely important, particularly for open-source models.

Different models can have different usage restrictions.

Some licenses are highly permissive.

Others may impose restrictions on:

-   Commercial use
-   Revenue
-   Redistribution
-   Modification
-   Other types of usage

Some models also require users to accept additional agreements.

Therefore:

> **Never assume that an open-source model can automatically be used for any commercial purpose.**

The model's license needs to be checked against the intended business use.

* * *

# 22\. Model Selection Is a Multi-Dimensional Problem

The important lesson is that choosing a model isn't about finding one number and picking the highest value.

You are balancing multiple dimensions:

```
                 Model Selection
                       │
       ┌───────────────┼────────────────┐
       │               │                │
  Capability         Cost             Speed
       │               │                │
  Reasoning        API/Compute       Latency
  Coding           Training          TTFT
       │               │                │
       └───────────────┼────────────────┘
                       │
                 Business Needs
                       │
          ┌────────────┼────────────┐
          │            │            │
      Context      Rate Limits    License
       Window
```

The best model is the one that provides the **right combination of characteristics for your specific requirements**.

* * *

# 23\. Practical Model Selection Workflow

A good workflow from this lesson is:

### Step 1 — Understand the task

Define exactly what the model needs to accomplish.

### Step 2 — Define requirements

Determine things such as:

-   Required intelligence
-   Speed requirements
-   Context requirements
-   Budget
-   Commercial requirements
-   Need for reasoning
-   Need for tool calling
-   Deployment requirements

### Step 3 — Build a candidate set

Use basic model information to eliminate models that don't fit.

```
All available models
        ↓
Basic requirements
        ↓
Affordable?
Fast enough?
Correct license?
Enough context?
Appropriate model type?
        ↓
Candidate models
```

### Step 4 — Compare benchmarks

Evaluate the remaining candidates on relevant benchmarks.

### Step 5 — Test against the actual task

Benchmarks aren't enough.

The ultimate question is:

> **How well does the model perform on my actual problem?**

* * *

# Key Takeaways

-   **There is no universally best LLM.**
-   The correct question is: **Which model is best for this particular task?**
-   Model selection should begin with **requirements**, not benchmark rankings.
-   Evaluate models at two levels:
    
    1.  Basic model characteristics
    2.  Benchmarks and real-world performance
-   Important basic characteristics include:
    
    -   Open vs closed source
    -   Chat vs reasoning vs hybrid
    -   Release date
    -   Knowledge cutoff
    -   Parameters
    -   Training tokens
    -   Context window
    -   Cost
    -   Training cost
    -   Build cost
    -   Time to market
    -   Rate limits
    -   Speed
    -   Latency / TTFT
    -   License
-   **Reasoning models aren't automatically better**; they can be slower and less suitable for some creative tasks.
-   **Larger parameter counts generally help**, but parameter count alone is not a reliable measure of model quality.
-   A model's **knowledge cutoff** tells you how recent its built-in training knowledge is.
-   Information beyond the cutoff can be supplied through **prompts or tools**.
-   A **context window** includes the conversation/context being supplied to the model, not merely the current prompt.
-   **Local models still have compute costs**, even when there is no API bill.
-   A cheaper model can have a higher **development cost** because it may require more engineering and experimentation.
-   **Time to market** is therefore part of model economics.
-   **Rate limits** can become important for production systems.
-   **Speed** and **latency** should both be evaluated.
-   **Time to First Token (TTFT)** is especially important for interactive and streaming applications.
-   **Licensing** must be checked carefully before using an open-source model commercially.
-   Model cards and leaderboards are useful sources for comparing basic model characteristics.
-   Ultimately, the right model is the one that provides the required **capability, cost, speed, context, reliability, and legal fit** for the task.