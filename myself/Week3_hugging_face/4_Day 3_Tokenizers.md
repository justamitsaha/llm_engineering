## 1\. Moving to the Lower-Level Hugging Face API

Week 3, Day 2 introduced **Hugging Face Pipelines**, the high-level API for quickly performing common inference tasks.

Day 3 moves to the lower-level side of Hugging Face.

The focus is on:

-   Tokenizers
-   Tokens
-   Token IDs
-   Special tokens
-   How text is converted into numbers
-   The relationship between tokenization and vectors

The overall progression is:

```
Pipelines
   ↓
Tokenizers
   ↓
Models
```

Pipelines make things easy. Tokenizers and models expose more of what is happening underneath.

* * *

# 2\. Why Do We Need Tokenizers?

A language model is fundamentally a **mathematical system**.

It does not directly work with words such as:

```
"Hello, how are you?"
```

Instead, it works with **numbers**.

A tokenizer provides the translation between natural language and the numerical representation that is fed into the model.

```
Natural language
       ↓
   Tokenizer
       ↓
   Token IDs
       ↓
     Model
```

* * *

# 3\. Tokenization Actually Has Two Steps

There is an important terminology distinction.

The process can be thought of as:

```
Text
 ↓
Tokens
 ↓
Token IDs
```

## Step 1 — Text → Tokens

The tokenizer breaks text into smaller pieces called **tokens**.

A token can be:

-   Part of a word
-   An entire word
-   Occasionally multiple words

For example, a sentence might be broken into several chunks:

```
"This is amazing"
       ↓
"This" | " is" | " amazing"
```

The exact tokenization depends on the tokenizer.

* * *

## Step 2 — Tokens → Token IDs

Every possible token has an associated number.

That number is called the **token ID**.

```
Token       Token ID
---------------------
"This"      123
"is"        456
"amazing"   789
```

So, strictly speaking:

> **Token = the piece of text**

> **Token ID = the number representing that piece of text**

People often use the word _token_ loosely when they actually mean token ID, but technically they are different things.

* * *

# 4\. The Tokenizer's Vocabulary

A tokenizer maintains a vocabulary containing the tokens it knows about.

Conceptually:

```
Vocabulary
│
├── token A → ID
├── token B → ID
├── token C → ID
├── ...
└── special tokens → IDs
```

When the tokenizer encounters a piece of text, it:

1.  Breaks the text into tokens.
2.  Looks up each token in its vocabulary.
3.  Replaces each token with its corresponding token ID.

The result is a sequence of numbers that can be passed into the model.

* * *

# 5\. Special Tokens

The vocabulary doesn't only contain tokens representing ordinary language.

It also contains **special tokens**.

Special tokens don't necessarily represent words in natural language. Instead, they communicate structural information to the model.

For example, there can be a special token representing:

> "This is the beginning of a prompt."

That special token has its own token ID.

Conceptually:

```
START_OF_PROMPT → 10
```

Whenever that token ID appears in the appropriate position, it represents the beginning of a prompt.

Other special tokens can represent other structural information.

* * *

# 6\. How Does a Special Token Have Meaning?

An important point is that the number itself has **no inherent meaning**.

Suppose:

```
START_OF_PROMPT → 10
```

The model doesn't inherently understand that:

```
10 = beginning of prompt
```

There is nothing magical about the number 10.

Instead, the model learns the meaning from its **training data**.

During training, the same special token ID is repeatedly used in situations where a prompt begins.

Over enormous amounts of training data, the neural network learns the statistical relationship between that token and what tends to follow it.

Conceptually:

```
Special token
     ↓
Repeated pattern in training data
     ↓
Model learns the relationship
     ↓
Better next-token predictions
```

* * *

# 7\. Special Tokens Are a Convention Learned During Training

This is an important conceptual distinction.

Special tokens are **not inherently understood by the Transformer architecture**.

The architecture doesn't contain a built-in rule saying:

```
"Token ID 10 means start of prompt."
```

Instead:

```
Token ID 10
     ↓
Used consistently during training
     ↓
Model observes the pattern
     ↓
Model learns what the pattern represents
```

The special token is useful because of **how it was used during training**.

This is similar to giving a model an input feature repeatedly associated with a particular outcome. With enough examples, the model learns the relationship.

* * *

# 8\. Different Models Can Have Different Tokenizers

There is **no universal tokenizer** shared by all LLMs.

Different models can have different tokenizers.

Examples mentioned in the lesson include:

-   Llama
-   Microsoft's models
-   DeepSeek
-   Qwen

Each model's creators can design a tokenizer that they believe works well for their model.

Therefore:

```
Model A → Tokenizer A
Model B → Tokenizer B
Model C → Tokenizer C
```

You should think of the tokenizer as being **part of the model ecosystem**, rather than assuming that all LLMs tokenize text in the same way.

* * *

# 9\. Why Do Different Models Produce Different Numbers of Tokens?

The same sentence can be divided differently by different tokenizers.

For example:

```
Sentence
   ↓
Tokenizer A → 8 tokens
Tokenizer B → 10 tokens
Tokenizer C → 7 tokens
```

This is normal.

The lesson specifically warns against getting too concerned about this.

A tokenizer producing fewer tokens might theoretically reduce input-token costs slightly, but that difference is generally much less important than the **quality of the model's output**.

Therefore:

> Don't get overly focused on why different tokenizers produce different token counts.

The important concept is simply:

> **A model has its own tokenizer.**

* * *

# 10\. Token IDs Are the Model's Input Representation

Statistical models operate on numbers.

Therefore, when text enters an LLM, it is first represented as token IDs.

```
Natural language
       ↓
     Tokens
       ↓
   Token IDs
       ↓
     Model
```

For example:

```
"Hello world"
      ↓
["Hello", " world"]
      ↓
[1234, 5678]
      ↓
      LLM
```

The specific IDs are determined by that model's tokenizer.

* * *

# 11\. Tokens Are Not Vectors

Another important distinction is between **tokens** and **vectors**.

They are not the same thing.

The process is more like:

```
Text
 ↓
Tokens
 ↓
Token IDs
 ↓
Embedding inside the model
 ↓
Vectors
```

Vectors are produced deeper within the neural network after token IDs have been embedded.

Therefore:

> **Tokens are not vectors.**

Token IDs are the initial numerical representation used to feed information into the model.

Vectors are a later representation inside the model.

* * *

# 12\. What Can Come Out of a Model?

Token IDs go **into** the model.

Different things can come **out**, depending on what you are doing.

For example, the output could be:

-   The next token
-   A vector embedding
-   Other model-specific outputs

The key point is:

```
INPUT
Token IDs
    ↓
  MODEL
    ↓
OUTPUT
Next token / vector / other result
```

For all these models, the initial input is represented using **token IDs**.

* * *

# 13\. Tokenization as a Prequel to Vectors

If you already know about vector embeddings, keep this sequence in mind:

```
Text
 ↓
Tokenization
 ↓
Token IDs
 ↓
Embedding
 ↓
Vectors
```

Therefore, tokenization happens **before** the vector representations discussed later in the course.

* * *

# 14\. Tokenizer Belongs to the Model

A useful mental model is:

```
        MODEL
          │
          ├── Tokenizer
          │
          └── Neural Network
```

The tokenizer is designed as part of the model's overall system.

You shouldn't assume that a tokenizer used by one model can simply be substituted for another model's tokenizer.

Different models can make different decisions about:

-   How text is split
-   Which tokens exist
-   Token IDs
-   Special tokens
-   How training data is represented

* * *

# 15\. Examples of Model-Specific Tokenizers

The lesson mentions several model families and their corresponding tokenizers:

```
Llama      → Llama tokenizer
Microsoft  → Microsoft model tokenizer
DeepSeek   → DeepSeek tokenizer
Qwen       → Qwen tokenizer
```

The course will experiment with these different tokenizers to demonstrate how they work.

* * *

# 16\. Where This Fits in the Course

Day 3 is essentially a **prequel to working directly with models**.

The progression is:

```
Day 1
Hugging Face Hub + Libraries
        ↓
Day 2
Pipelines
        ↓
Day 3
Tokenizers
        ↓
Day 4
Models
```

The reason for learning tokenizers first is that understanding how text becomes numbers is necessary before working directly with the model itself.

* * *

# Key Takeaways

-   Day 3 moves from Hugging Face's **high-level Pipelines API** toward the lower-level API.
-   **Tokenizers translate natural language into numerical representations.**
-   Tokenization can be understood as two steps:
    
    1.  Text → tokens
    2.  Tokens → token IDs
-   A **token** is a piece/chunk of text.
-   A **token ID** is the number representing that token.
-   The tokenizer maintains a vocabulary mapping tokens to IDs.
-   Tokenizers also contain **special tokens** used to represent structural information.
-   A special token has meaning because the model **learned its usage from training data**, not because the number itself has inherent meaning.
-   Different LLMs can have completely different tokenizers.
-   Different tokenizers can produce different numbers of tokens for the same text.
-   Token count differences are generally less important than the quality of the model's output.
-   **Tokens and vectors are not the same thing.**
-   Token IDs are the initial numerical inputs to the model.
-   Vectors are produced later, after the token IDs are embedded inside the neural network.
-   The next major step is to move from **tokenizers to models** and work with the underlying model directly.