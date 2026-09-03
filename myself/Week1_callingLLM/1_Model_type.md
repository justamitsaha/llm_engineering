# Frontier Models vs Open-Source Models

## 1\. What Are LLMs?

LLMs are the models that generate content and power **generative AI**.

The most capable models are often described as:

> **Frontier models**

The companies developing these models are informally called:

> **Frontier Labs**

There is no strict technical definition of "frontier." It generally refers to the largest and most capable models being developed by leading AI companies.

* * *

# 2\. Closed-Source vs Open-Source Models

A major distinction between models is whether they are **closed source** or **open source**.

## Closed-Source Models

These are models developed by companies that generally:

-   Keep the model implementation and/or training details private.
-   Provide access primarily through products or APIs.
-   Charge for API usage.

They are often the models being referred to when people say:

> **Frontier models**

Examples include:

-   GPT
-   Claude
-   Gemini
-   Grok

The companies behind them spend enormous amounts of money training and operating these models, which contributes to their API costs.

* * *

# 3\. Open-Source Models

Open-source models are models that can be accessed and run by others.

A major example is:

> **Llama**

Llama is developed by Meta.

The lesson notes an important terminology issue: technically, some people argue that models such as Llama are better described as **open-weight** rather than fully open-source.

Why?

Because although the model weights are made available, the complete:

-   Training data
-   Training methodology
-   Other development details

may not necessarily be publicly available.

Nevertheless, the industry commonly refers to these models as **open-source models**.

* * *

# 4\. What Does "Frontier" Usually Mean?

In most contexts in this course:

```
Frontier Model
      ↓
Large, highly capable
      ↓
Usually closed-source
      ↓
Accessed through products/APIs
```

However, the term **frontier** can sometimes also be used for open-source models.

Therefore, always consider the context in which someone uses the term.

* * *

# 5\. The Major Frontier Labs

The lesson focuses on the **four major frontier model providers**:

```
OpenAI
Anthropic
Google
xAI
```

Their major models are:

```
OpenAI    → GPT
Anthropic → Claude
Google    → Gemini
xAI       → Grok
```

These are described as the **"big four"** frontier model families.

The course will use and compare all four.

* * *

# 6\. OpenAI — GPT

OpenAI is described as the most prominent AI lab in the ecosystem.

Its most famous model family is:

> **GPT**

A major turning point came with **ChatGPT**, which initially used GPT-3.5 in late 2022.

ChatGPT significantly changed public perception of what LLMs could do.

Since then, OpenAI has released many generations of GPT models.

The lesson describes GPT-5 as the current generation at the time the material was recorded.

* * *

# 7\. The O-Series

OpenAI also developed another family of models known as the **o-series**.

Examples mentioned include:

-   o1
-   o3
-   o4-mini

The lesson explains that the GPT and o-series have increasingly converged, with newer GPT models incorporating capabilities associated with both approaches.

* * *

# 8\. Anthropic — Claude

Anthropic is another major frontier AI lab.

Anthropic was founded by people who had previously worked at OpenAI.

Its primary model family is:

> **Claude**

Claude comes in different model sizes:

```
Haiku
  ↓
Small

Sonnet
  ↓
Middle / highly capable

Opus
  ↓
Large
```

The lesson describes **Sonnet** as particularly powerful and competitive with GPT.

Claude is also the model behind:

> **Claude Code**

Claude Code is a coding-agent platform built around Claude.

* * *

# 9\. Google — Gemini

Google develops:

> **Gemini**

Gemini is Google's major closed-source model family.

The lesson connects Gemini with **Gemma**, Google's open-source model family:

```
Google
├── Gemini → Closed-source
└── Gemma  → Open-source
```

The lesson notes that Google initially appeared to be behind companies such as OpenAI and Anthropic.

Google's early chatbot **Bard** received significant criticism.

However, Google subsequently made substantial progress and Gemini became a major competitor.

* * *

# 10\. Gemini Model Family

The lesson mentions:

> **Gemini 2.5 Pro**

as the current model at the time of recording.

It also anticipates the release of **Gemini 3**.

Google also provides smaller Gemini models, some of which may be available at low or no cost for students depending on the user's region.

* * *

# 11\. xAI — Grok

The fourth major frontier model family is:

> **Grok**

Grok is developed by **xAI**, the AI company founded by Elon Musk.

It is associated with the X ecosystem.

The lesson considers Grok the fourth member of the major frontier-model group:

```
The Big Four

OpenAI    → GPT
Anthropic → Claude
Google    → Gemini
xAI       → Grok
```

* * *

# 12\. Other Frontier Models

The "big four" aren't the only important models.

The lesson also mentions several other companies and model families, including:

-   **Perplexity**
-   **Cohere**
-   **Mistral**

Mistral is particularly interesting because it has both:

-   Closed-source models
-   Open-source models

These companies are described as part of the broader group of significant AI model providers beyond the biggest four.

* * *

# 13\. Comparing the Major Models

A useful high-level map is:

| Company | Model Family | Type |
| --- | --- | --- |
| OpenAI | GPT | Closed-source / frontier |
| Anthropic | Claude | Closed-source / frontier |
| Google | Gemini | Closed-source / frontier |
| xAI | Grok | Closed-source / frontier |
| Meta | Llama | Open-source / open-weight |
| Google | Gemma | Open-source |
| Mistral | Mistral models | Both |

* * *

# 14\. Frontier Models vs Open-Source Models

The central distinction can be summarized as:

### Frontier / Closed-Source

```
AI Company
    ↓
Large investment
    ↓
Train massive model
    ↓
Keep model details largely private
    ↓
Provide access through products/APIs
```

### Open-Source / Open-Weight

```
AI Company / Organization
    ↓
Train model
    ↓
Release weights / model
    ↓
Developers can access and run it
    ↓
Can often customize and experiment
```

This distinction becomes particularly important because the course has already introduced Hugging Face and its open-source ecosystem.

* * *

# 15\. Why We Need Multiple Models

There isn't necessarily one model that is best for every task.

Different models can vary in:

-   Intelligence
-   Coding ability
-   Reasoning
-   Speed
-   Cost
-   Context window
-   Multimodal capabilities
-   Availability
-   Open vs closed access

The course will therefore spend **Week 4** specifically looking at:

> **How to choose the right LLM for a particular task.**

* * *

# 16\. Course Strategy

The course will work with all four major frontier model families:

```
GPT
Claude
Gemini
Grok
```

You don't necessarily need API access to every one of them.

The course can demonstrate some models directly, while you can follow along with the others.

The important goal is to understand how these models compare rather than becoming dependent on one particular provider.

* * *

# Key Takeaways

-   **LLMs** are the models that generate content for generative-AI applications.
-   The most capable models are often called **frontier models**.
-   The companies developing them are informally called **frontier labs**.
-   "Frontier" has no strict technical definition.
-   In most contexts, frontier models refer to the leading **closed-source models**.
-   **Open-source/open-weight models** make model weights available so developers can run them themselves.
-   Llama is a major example of an open-source/open-weight model from Meta.
-   Some people prefer the term **open-weight** because the complete training data and methodology may not be released.
-   The four major frontier model families discussed are:

```
OpenAI    → GPT
Anthropic → Claude
Google    → Gemini
xAI       → Grok
```

-   Claude comes in families such as **Haiku, Sonnet, and Opus**.
-   Google's **Gemini** is its closed-source model family, while **Gemma** is its open-source model family.
-   **Grok** is developed by xAI.
-   Other important model providers include Perplexity, Cohere, and Mistral.
-   Mistral has both closed-source and open-source models.
-   The course will compare the major frontier models and later examine **how to choose the right model for a particular task**.