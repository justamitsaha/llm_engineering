## Week 2 

### Day 1
 ### 1.  **Inference vs Training Scaling**  
    With small model and no resoning it will answer incorrect. You dial up the reasoning(Inference scaling) and it gets it right. You dial up the model size(Training Scaling) and it gets it right. And this is showing you how you can scale at at training time with a bigger model, or at inference time with more reasoning.
### 2.  **Groq** 
    grok with a Q is a, are a vendor. They're a provider that runs open source models on the cloud. They have a cloud setup. They will run large open source models too big to run on your own computer like gpt-oss-120 big open source models. They'll do it on the cloud, but there's something special about them. They have custom built, some very efficient hardware that's super fast at running these kinds of models.And as a result, you can run your model on grok with a hardware and it will go it will blaze through at a very fast pace.

### 3. **llama3.2, gpt-oss:20b**: 
Only do this if you have a large machine - at least 16GB RAM

### 4. **Routers vs Abtraction Layers**:
Routers like open router help to route the request to different providers. Abstraction layers like Langchain and LiteLLM  provide aditional feartues as abstraction.

### 5. Prompt Caching Summary

When you repeatedly send the **same or very similar input context** within a short period, the provider can reuse work it has already done instead of processing the entire prompt again. This significantly reduces the cost of subsequent requests.

#### Example from the text

-   A large prompt containing the **entire text of Hamlet** is sent to the model.
-   The **first request** processes all ~53,000 input tokens and costs about **0.5 cents**.
-   The **second request**, made shortly afterward with the same context, uses **cached tokens** (about 52,200 of the 53,000 input tokens), reducing the cost to roughly **0.1 cents**—about **5× cheaper**.
    
#### How to maximize cache hits

The beginning of the prompt should remain **identical** across requests.

Good structure:

```
[Large static context]
[User question]
```

Bad structure:

```
[User question]
[Large static context]
```

Similarly, dynamic values such as the current date or timestamp should be placed **near the end** of the prompt rather than at the beginning. If the first part of the prompt changes, prompt caching may not work.


#### Provider differences

-   **OpenAI:** Prompt caching is generally automatic when the initial portion of the prompt matches exactly.
-   **Anthropic:** Caching must be explicitly enabled ("prime the cache"). Priming costs about **25% more**, but subsequent cached requests are about **10× cheaper**.
-   **Gemini:** Supports both implicit (automatic) and explicit caching modes.
    
    

#### Why it matters

Prompt caching is most valuable when:

-   You repeatedly send large documents (manuals, books, codebases, knowledge bases).
-   Multiple users query the same reference material.
-   You want to reduce API costs while maintaining the same quality of responses.

The text also highlights that tools like **LiteLLM** make caching easy to observe by reporting:

-   Total input/output tokens
-   Cached input tokens
-   Cost per API call

This makes it practical to monitor API spending and optimize the economics of production AI applications.