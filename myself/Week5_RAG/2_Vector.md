# Vectors, Embeddings, and Encoder Models

## 1\. Why Vectors Matter

**Vectors are a fundamental idea behind RAG (Retrieval-Augmented Generation).**

They allow us to perform **fuzzy lookup**—given a user's question, we can find information that is **semantically related** to that question, even when the exact words don't match.

```
User question
      ↓
Convert meaning into a vector
      ↓
Compare with other vectors
      ↓
Find semantically similar information
      ↓
Relevant context
```

This is the foundation of how vector embeddings are used in RAG.

* * *

# 2\. Two Broad Types of Language Models

The lesson explains that there are two broad types of language models, although most of the LLMs encountered so far belong to one category.

The most common type is the **autoregressive language model**.

The other type is commonly called:

-   **Encoder**
-   **Embedding model**
-   **Vector embedding model**
-   **Autoencoder** in the lesson's terminology

* * *

# 3\. Autoregressive Language Models

An **autoregressive LM** is trained primarily to predict the **next token** given the tokens that came before it.

For example:

```
Input:
"The cat sat on the"

        ↓

Predict next token:
"mat"
```

Then the predicted token is added to the input:

```
"The cat sat on the mat"
              ↓
        Predict next token
```

This process repeats one token at a time.

```
Input
 ↓
Predict next token
 ↓
Add token to input
 ↓
Predict next token
 ↓
Add token
 ↓
Repeat...
```

The model is therefore **regressive** because it looks back at the existing sequence to determine what should come next.

Most commonly encountered LLMs, when someone simply says "language model," are this type.

* * *

# 4\. Encoder / Embedding Models

The other type works differently.

Instead of predicting the next token, an **encoder/embedding model** takes the **whole input sequence** and produces an output that represents the input as a whole.

Conceptually:

```
Full input sequence
        ↓
     Encoder
        ↓
Representation of the input
```

The output is intended to capture something about the **meaning of the entire input**, rather than simply predicting what comes next.

* * *

# 5\. What Are Encoder Models Used For?

One obvious application is **classification**.

For example, given:

```
"I absolutely loved this movie!"
```

the model could classify it as:

```
Positive sentiment
```

Similarly, an encoder could classify text according to what it is about.

```
Input text
    ↓
Encoder
    ↓
Classification
```

Other tasks can also use this general idea whenever you want **one output that represents the entire input**.

* * *

# 6\. From Text to a Vector

The particularly important application for us is producing a **set of numbers that represents the meaning of the input**.

For example:

```
Input:
"The dog is playing in the garden."

             ↓
        Embedding model
             ↓

[0.21, -0.73, 0.14, 0.82, ...]
```

These numbers are called a **vector embedding**.

The numbers collectively represent the semantic information contained in the input.

* * *

# 7\. Why Is It Called a Vector?

In mathematics, a vector can be thought of as a **set of numbers representing a point in space**.

For example:

```
[3, 5, 2]
```

can represent a point in three-dimensional space:

```
X = 3
Y = 5
Z = 2
```

If there are ten numbers:

```
[0.2, 0.7, -0.4, ...]
```

then it can be thought of as a point in **10-dimensional space**.

So:

```
Input text
    ↓
Set of numbers
    ↓
Vector
```

That vector is the **embedding** of the input.

* * *

# 8\. Examples of Embedding Models

The lesson gives several examples:

### BERT

**BERT** was created by Google in 2018.

Its name stands for:

> **Bidirectional Encoder Representations from Transformers**

It predates GPT and became an important early example of this type of model.

### OpenAI Embeddings

OpenAI provides embedding models, including examples such as:

-   **text-embedding-3-large**
-   **text-embedding-3-small**

### Open-Source Embeddings

The lesson also mentions:

-   **all-MiniLM-L6-v2**

These models can take text and transform it into vectors.

* * *

# 9\. Tokens vs. Vectors

A common source of confusion is the difference between **tokens** and **vectors**.

The simplest distinction is:

> **Tokens are inputs; vectors are outputs.**

```
Text
 ↓
Tokens
 ↓
Model
 ↓
Vector
```

But they represent fundamentally different things.

* * *

# 10\. What Are Tokens?

A model is fundamentally a mathematical system—it cannot directly process ordinary words as words.

So text must first be converted into **numbers**.

For example, conceptually:

```
"the" → some numeric token
"cat" → some numeric token
"sat" → some numeric token
```

However, modern tokenization generally doesn't assign one token to every complete word.

Instead, text is divided into **fragments/subwords**.

```
Text
 ↓
Word fragments / tokens
 ↓
Numeric representation
```

This provides a useful compromise between having a token for every word and having a token for every individual character.

Tokenizers therefore perform a relatively straightforward mapping from text into tokens.

* * *

# 11\. What Are Vectors?

Vectors are much richer representations.

After the input tokens enter the model, the model processes the relationships and meaning represented by those tokens.

An embedding model can then produce a vector:

```
Tokens
   ↓
Embedding model
   ↓
Semantic representation
   ↓
Vector
```

So while tokens are primarily a **numeric representation of the input**, vectors are a **numeric representation of information/meaning derived from the input**.

* * *

# 12\. The Important Difference

A useful mental model is:

```
TEXT
  ↓
TOKENS
  ↓
MODEL
  ↓
VECTOR
```

### Tokens

-   Represent the input text numerically
-   Are produced by a tokenizer
-   Are used as inputs to the model

### Vectors

-   Are produced by the model
-   Contain a representation of the information/meaning
-   Can be treated mathematically as points in a high-dimensional space

Therefore:

> **Tokens tell the model what the input is in numeric form; vectors represent information about what that input means.**

* * *

# 13\. Vectors Also Exist Inside LLMs

There is an important advanced qualification.

Vectors aren't **only** final outputs.

Inside a transformer, information is represented and passed between different layers using numerical representations that can also be thought of as vectors.

Even autoregressive models internally transform tokens into richer vector representations.

However, for now, the most useful distinction is simply:

```
Tokens
= simple numeric representation of text/input

Vectors
= richer numerical representation of information/meaning
```

* * *

# 14\. Connection to RAG

This is the key reason vectors are important for the course.

RAG needs to answer a question like:

> **"Which pieces of information are relevant to this question?"**

Exact keyword matching isn't always enough.

Instead, text can be converted into vectors representing its meaning:

```
Documents
   ↓
Embedding model
   ↓
Document vectors
```

Then the user's question can also be converted into a vector:

```
User question
   ↓
Embedding model
   ↓
Question vector
```

The system can then compare the vectors to find **semantically related information**.

```
Question vector
      ↓
Compare with document vectors
      ↓
Find similar vectors
      ↓
Relevant context
      ↓
RAG
```

This is the foundation of **fuzzy/semantic retrieval in RAG**.

* * *

# Key Takeaways

-   **Vectors are a core concept behind RAG and semantic/fuzzy lookup.**
-   There are two broad types of language models discussed:
    
    -   **Autoregressive models**
    -   **Encoder/embedding models**
-   **Autoregressive LMs** predict the next token one token at a time.
-   **Encoder/embedding models** process the full input and produce a representation of it.
-   An **embedding** is a set of numbers representing information/meaning from an input.
-   A set of numbers can be treated mathematically as a **vector**, or a point in high-dimensional space.
-   Examples of embedding models include **BERT**, **text-embedding-3-large**, **text-embedding-3-small**, and **all-MiniLM-L6-v2**.
-   **Tokens and vectors are not the same thing**:

```
Text → Tokens → Model → Vector
```

-   Tokens are primarily a **numeric representation of input text**.
-   Vectors are a **richer representation of information/meaning**.
-   Vectors can also occur as internal representations throughout transformer models.
-   The key RAG idea is to represent both **documents and questions as vectors**, then use their relationships to find relevant context.

### One-line memory aid

> **Tokens represent the input; embeddings turn that input into vectors that represent its meaning—and those vectors are what make semantic retrieval in RAG possible.**