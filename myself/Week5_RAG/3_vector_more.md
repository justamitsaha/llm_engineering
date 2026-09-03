# How Vectors Represent Meaning

## 1\. What Does It Mean for Numbers to Represent Meaning?

An **embedding model** takes an input and converts it into a set of numbers called a **vector embedding**.

But the important question is:

> **How can a collection of numbers represent meaning?**

Vectors can represent many kinds of things:

-   Individual characters
-   Tokens
-   Words
-   Sentences
-   Paragraphs
-   Documents
-   Abstract concepts

For example, vectors can even represent **job opportunities** or **people's careers**, showing that embeddings aren't limited to ordinary text.

* * *

# 2\. Vectors Can Have Many Dimensions

A vector doesn't have to contain only three numbers.

For example:

```
3 dimensions:
[X, Y, Z]
```

But an embedding could contain:

```
100 dimensions
1000 dimensions
Thousands of dimensions
```

A 1,000-dimensional vector simply means:

> **1,000 numbers are being used to represent the input.**

You can think of the vector as a point in a **1,000-dimensional space**.

* * *

# 3\. Similar Meaning → Similar Vectors

The fundamental idea is:

> **Inputs with similar meanings should have vectors that are close to each other.**

Imagine a simple 3-dimensional vector space.

Consider:

```
"What are ticket prices to travel from New York to London?"
```

and:

```
"What's the price of a flight from JFK to Heathrow Airport?"
```

The words are quite different, but the **meaning is very similar**.

Therefore, their embeddings should be located close together:

```
Sentence A ●
            \
             \  similar meaning
              \
               ● Sentence B
```

The embedding model is trained so that its vectors have this useful property.

* * *

# 4\. Semantic Similarity

This gives us a powerful way of comparing text.

Instead of asking:

> "Do these two sentences contain the same words?"

we can ask:

> **"Do these two sentences have similar meanings?"**

For example:

```
"How much does a flight from New York to London cost?"
                    ↕
"What is the price of travelling from JFK to Heathrow?"
```

Different wording, but similar meaning → **similar embeddings**.

This is the fundamental idea behind **semantic search** and is extremely important for RAG.

* * *

# 5\. Word2Vec: An Early Example

The lesson introduces **Word2Vec**, an earlier technique from Natural Language Processing (NLP).

Word2Vec mapped individual words to vectors.

For example:

```
"orange"
   ↓
[vector of numbers]
```

Words with related meanings tended to appear close together in vector space.

For example:

```
             orange
            /   |   \
     mandarin  fruit  color
       /
  tangerine
```

This was an early demonstration that vectors could capture meaningful relationships between words.

* * *

# 6\. The Surprising Power of Vector Arithmetic

One of the most fascinating discoveries with Word2Vec was that you could perform **mathematical operations on meanings**.

For example:

```
KING - MAN + WOMAN ≈ QUEEN
```

Conceptually:

```
King
  ↓
Remove "man"
  ↓
Add "woman"
  ↓
Queen
```

The resulting vector is located close to the vector for **queen**.

This means that the vector space isn't merely grouping similar words—it can also capture **relationships between concepts**.

* * *

# 7\. Another Example: Paris → London

A similar example is:

```
PARIS - FRANCE + ENGLAND ≈ LONDON
```

Conceptually:

```
Paris
  ↓
Remove France
  ↓
Add England
  ↓
London
```

The resulting vector is close to the vector representing **London**.

This can feel surprising the first time you see it because the model isn't explicitly performing a symbolic rule like:

```
France → Paris
England → London
```

Instead, the **geometry of the vector space** captures these relationships.

* * *

# 8\. Meaning Can Be Manipulated

This leads to an important idea:

> **Vector space can represent relationships between meanings, not just individual meanings.**

You can think of vector operations as:

```
Existing meaning
       ↓
Remove one concept
       ↓
Add another concept
       ↓
New meaning
```

With Word2Vec, this was demonstrated primarily with individual words.

Modern encoder models extend the idea much further.

* * *

# 9\. Modern Embeddings Represent Entire Passages

Modern encoder/embedding models can represent much larger inputs:

```
Word
 ↓
Sentence
 ↓
Paragraph
 ↓
Document
```

For example:

```
Paragraph
    ↓
Embedding model
    ↓
Vector
```

The vector represents the **meaning of the paragraph as a whole**.

You can conceptually manipulate the meaning represented by that vector and move toward another representation.

The important point is that it is the **meaning**, rather than simply the specific words, that matters.

* * *

# 10\. How This Enables Fuzzy Search

This is the key connection to RAG.

Suppose a user asks:

```
"How much is a flight from New York to London?"
```

An embedding model converts it into a vector:

```
Question
   ↓
Embedding model
   ↓
Question vector
```

Documents are also converted into vectors:

```
Documents
   ↓
Embedding model
   ↓
Document vectors
```

The system then looks for vectors that are **semantically similar**.

```
             Question
                ●
               / \
              /   \
             ●     ●
        Relevant   Less relevant
        document   document
```

This allows RAG to retrieve relevant information even when the document uses **different words from the user's question**.

* * *

# 11\. Cosine Similarity

There is one technical qualification.

It isn't quite correct to say that we simply measure whether two vectors are physically "close" to each other.

In practice, a common method is **cosine similarity**.

Conceptually:

```
Vector A
   ↘
    angle between vectors
   ↗
Vector B
```

Cosine similarity measures how similarly the vectors are oriented.

For this lesson, the important idea is simply:

> **Higher semantic similarity → higher vector similarity.**

The mathematical details of cosine similarity can be studied later.

* * *

# Key Takeaways

-   **Embeddings represent meaning using numbers.**
-   A vector can represent:
    
    -   Words
    -   Sentences
    -   Paragraphs
    -   Documents
    -   Even abstract concepts
-   Embeddings can have **hundreds or thousands of dimensions**.
-   The fundamental principle is:

```
Similar meaning
      ↓
Similar vectors
```

-   Different sentences can have completely different words but still produce similar vectors if their meanings are similar.
-   **Word2Vec** was an early example of mapping words into meaningful vector spaces.
-   Vector arithmetic can capture relationships such as:

```
KING - MAN + WOMAN ≈ QUEEN
```

and:

```
PARIS - FRANCE + ENGLAND ≈ LONDON
```

-   Modern encoder models extend this idea from individual words to **sentences, paragraphs, and documents**.
-   This semantic representation is what makes **fuzzy/semantic retrieval** possible.
-   **Cosine similarity** is commonly used to determine how similar two vectors are.

### One-line memory aid

> **An embedding turns meaning into a point in high-dimensional space, where similar meanings have similar vector representations—making semantic search and RAG possible.**