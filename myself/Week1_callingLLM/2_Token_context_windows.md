# Context Windows and API Costs

This section introduces two important practical constraints when working with LLMs:

1.  **Context windows** — how much information a model can consider at once.
2.  **API costs** — how much it costs to process that information and generate responses.

These concepts become especially important when building applications and agents at scale.

* * *

# 1\. Context Window

The **context window** is the maximum number of tokens a model can process as input when generating its next token.

In simple terms:

> **Context window = the maximum amount of information the model can consider at one time.**

If you send more tokens than the model's context window allows, the request will fail.

```
Input tokens
     ↓
┌─────────────────────┐
│   Context Window    │
│                     │
│  Maximum capacity   │
└─────────────────────┘
     ↓
Next-token generation
```

* * *

# 2\. The Context Is More Than Your Latest Message

An important point is that the context isn't simply the most recent message.

For a conversation, the model may receive the **entire conversation so far**.

For example:

```
User: Hi, my name is Ed.
Assistant: Nice to meet you, Ed.
User: What's my name?
```

The model needs the previous messages to answer correctly.

Therefore, the context can include:

-   Earlier user messages
-   Earlier assistant responses
-   The latest user message
-   Other information inserted into the prompt
-   Generated output so far

* * *

# 3\. Why Generated Tokens Also Matter

LLMs generate output **one token at a time**.

For example:

```
Input:
"What is my name?"

Model generates:
"Your"
     ↓
"Your name"
     ↓
"Your name is"
     ↓
"Your name is Ed"
```

Conceptually, the model repeatedly uses the existing sequence to predict the next token.

Therefore, as generation continues, the sequence being considered becomes larger.

The context window has to accommodate the relevant input and generated sequence.

* * *

# 4\. Context Window and "Memory"

The context window determines how much information the model can effectively refer back to during a conversation.

A larger context window allows you to provide more:

-   Conversation history
-   Documents
-   Examples
-   Retrieved information
-   Other background context

This is one reason context windows matter so much when building LLM applications.

* * *

# 5\. Multi-Shot Prompting

A particularly relevant technique is **multi-shot prompting**.

Instead of simply giving the model an instruction, you provide several examples of the desired behavior.

Conceptually:

```
Example 1:
Question → Answer

Example 2:
Question → Answer

Example 3:
Question → Answer

New Question:
Question → ?
```

The model uses those examples as context when determining what output to generate.

The more examples you provide, the more context-window space they consume.

* * *

# 6\. RAG Also Uses the Context Window

**RAG (Retrieval-Augmented Generation)** also makes significant use of the context window.

A simplified RAG workflow is:

```
User Question
     ↓
Retrieve relevant information
     ↓
Insert information into prompt
     ↓
LLM
     ↓
Answer
```

The retrieved information becomes part of the model's input.

Therefore, RAG consumes context-window capacity.

This is one reason context-window size is important when working with large documents or large retrieval results.

* * *

# 7\. Large Context Windows

The lesson gives the example of trying to provide the **complete works of Shakespeare** as context.

That requires an extremely large context window.

Models differ significantly in how much context they can handle.

The lesson gives examples such as:

| Model | Context Window |
| --- | --- |
| GPT-5 | 400,000 tokens |
| Claude range | 200,000 tokens |
| GPT open-source model mentioned | ~130,000 tokens |
| Gemini 2.5 Flash | 1,000,000 tokens |

The key takeaway is that context-window sizes vary considerably between models.

Pasted text

* * *

# 8\. API Costs

The second major topic is **API pricing**.

Chat products such as ChatGPT generally operate differently from APIs.

### Chat product

You typically pay a subscription:

```
Monthly subscription
        ↓
Use the product
        ↓
Subject to usage/rate limits
```

### API

With an API:

```
Your application
      ↓
API request
      ↓
Model inference
      ↓
You pay for usage
```

Having a subscription to a chat product does **not** mean API calls are free.

* * *

# 9\. Why API Calls Cost Money

Running an LLM requires enormous amounts of computation.

The model performs huge numbers of mathematical operations during inference.

API pricing therefore contributes toward the computational resources required to process your request.

The lesson also notes that model providers have substantial training costs that must be recovered.

* * *

# 10\. What Determines API Cost?

API costs are typically based primarily on:

```
Input tokens
+
Output tokens
=
Billable usage
```

Therefore, both what you **send** and what the model **generates** matter.

* * *

# 11\. Input Token Costs Include the Context

One important consequence is that input-token costs can include the **entire sequence being provided to the model**.

That means the input can include:

-   Conversation history
-   Earlier messages
-   Retrieved RAG information
-   Examples used for prompting
-   Other context inserted into the request

For example:

```
Conversation history
       +
RAG documents
       +
New user message
       ↓
Input tokens
       ↓
API cost
```

This can cause costs to increase as conversations become longer.

* * *

# 12\. Why Not Simply Forget the History?

You could send only the latest message.

For example:

```
Previous conversation → discarded
Latest message → sent
```

That would reduce the amount of input computation.

However, the model would lose the information contained in the earlier conversation.

Therefore:

```
More context
→ More computation
→ Higher cost
→ Better ability to use history

Less context
→ Less computation
→ Lower cost
→ Less information available
```

The trade-off is therefore deliberate.

* * *

# 13\. Reasoning Models and Output Costs

There is another important cost consideration with **reasoning models**.

A reasoning model may perform additional internal reasoning before producing its final answer.

Those reasoning tokens can count toward output usage and therefore API costs.

In some cases, the user doesn't see the internal reasoning, but the computation still has to happen.

This creates an important practical consideration:

> **You may pay for model processing that isn't necessarily visible in the final response.**

It can also make costs less predictable when reasoning workloads vary.

* * *

# 14\. API Cost Example: GPT-5

The lesson gives a pricing example for GPT-5:

-   Context window: **400,000 tokens**
-   Input: **$1.25 per million tokens**
-   Output: **$10 per million tokens**

The critical detail is:

> These prices are **per million tokens**, not per individual token.

So generating a very large amount of text can still be relatively inexpensive on a per-token basis, even though the total cost can become significant at scale.

* * *

# 15\. GPT-5 Nano

The lesson contrasts this with a much smaller model, **GPT-5 Nano**.

The figures given are:

-   Input: **$0.05 per million tokens**
-   Output: **$0.40 per million tokens**

This illustrates the significant difference in cost between larger and smaller models.

* * *

# 16\. Individual Usage vs Application-at-Scale

For an individual experimenting with an API, small requests can be extremely inexpensive.

For example:

```
"Hi, my name is Ed."
```

contains very few tokens.

The cost of processing a handful of tokens is tiny compared with the price per million tokens.

However, the economics change when building a production system.

For example:

```
Thousands of users
       ×
Many conversations
       ×
Large context
       ×
Many generated tokens
       ↓
Significant infrastructure cost
```

Therefore, when building an application, you need to understand your **unit cost per user/request**.

* * *

# 17\. Caching

The lesson also introduces **caching**.

If the same input is sent repeatedly within a relatively short period, some providers can cache parts of that information.

This can reduce input costs.

Conceptually:

```
First request
     ↓
Process normally

Same input again
     ↓
Cached information
     ↓
Lower input cost
```

The exact behavior differs between providers.

The lesson notes that caching is automatic in some cases, while with other providers such as Claude, the situation is different and more dependent on how caching is configured.

* * *

# 18\. Context Window vs Cost

These two concepts are closely related.

A larger context can be useful because the model can consider more information.

But larger context also means:

```
More tokens
     ↓
More computation
     ↓
Potentially higher API cost
```

This becomes especially important for applications that maintain long conversations or repeatedly send large documents.

* * *

# 19\. Why This Matters for AI Agents

The lesson's ideas become particularly important when building **agentic systems**.

An agent may repeatedly call an LLM:

```
Agent
 ↓
LLM call
 ↓
Tool
 ↓
LLM call
 ↓
Tool
 ↓
LLM call
 ↓
...
```

Each call can consume input and output tokens.

If the context keeps growing, the amount of information processed can become substantial.

Therefore:

> **Agent loops can consume tokens much faster than simple one-off prompts.**

This makes cost management important when building systems at scale.

* * *

# 20\. Leaderboards

The lesson introduces **LLM leaderboards** as another useful resource.

A leaderboard provides ranked comparisons between models.

These can help compare things such as:

-   Model performance
-   Context-window size
-   API pricing
-   Other capabilities

One leaderboard mentioned in the lesson is **Vellum**, which provides several different comparisons.

* * *

# 21\. The Bigger Picture

At this point, the course has established several foundational concepts:

```
Frontier Models
      ↓
Open-source Models
      ↓
Transformers
      ↓
Tokens
      ↓
Context Windows
      ↓
API Costs
```

These concepts form the foundation for building more practical LLM applications.

* * *

# What Comes Next

The next part of the course becomes more hands-on.

The upcoming material will cover:

-   OpenAI API
-   Chat Completions API
-   One-shot prompting
-   Streaming
-   Markdown results
-   JSON results
-   Building a practical business solution quickly
-   The illusion of model "memory"

The emphasis shifts from understanding concepts toward actually **building with APIs**.

* * *

# Key Takeaways

### Context Windows

-   A context window is the maximum amount of tokenized information a model can process at once.
-   It includes more than just the latest user message.
-   Conversation history can consume context.
-   Generated tokens also contribute to the sequence being processed.
-   Larger context windows allow models to work with more information.
-   **Multi-shot prompting** consumes context because examples are included in the input.
-   **RAG** also consumes context because retrieved information is inserted into the prompt.
-   Different models have dramatically different context-window sizes.

### API Costs

-   Chat-product subscriptions and API usage are separate.
-   API usage is generally charged based on **input and output tokens**.
-   Long conversation histories increase input-token usage.
-   RAG content also contributes to input-token usage.
-   Reasoning models can incur additional output-token costs from their reasoning process.
-   Small individual API calls are generally inexpensive.
-   At production scale, token usage becomes an important **unit-economics** consideration.
-   Smaller models can be dramatically cheaper than larger models.
-   **Caching** can reduce costs when the same inputs are repeatedly processed.
-   LLM leaderboards can help compare model capabilities, context windows, and pricing.

### Core Mental Model

```
             CONTEXT WINDOW
                   │
       ┌───────────┴───────────┐
       │                       │
Conversation              RAG / Examples
history                   / Documents
       │                       │
       └───────────┬───────────┘
                   ↓
              Input tokens
                   ↓
                 LLM
                   ↓
             Output tokens
                   ↓
              API cost
```