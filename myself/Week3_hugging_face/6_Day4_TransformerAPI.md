## 1\. Today's Focus

This lesson moves from the **high-level Hugging Face Pipelines API** to the **low-level Transformers API**.

Previously:

```
Pipelines
→ Simple interface for common tasks

Tokenizers
→ Convert text into token IDs
```

Now:

```
Transformers
→ Work directly with the model
→ Access the underlying PyTorch implementation
```

The model being used is **Llama**, and the goal is to understand how to load, quantize, tokenize, and run an actual neural network on a GPU.

* * *

# 2\. Google Colab GPU Setup

The exercises are performed in **Google Colab**.

The notebook should be connected to a **T4 GPU**.

You can verify this through the runtime/resources information and check that GPU memory is available.

The lesson expects roughly **15 GB of GPU RAM** to be available initially.

### If you encounter a CUDA error

If you see an error indicating that CUDA is unavailable:

1.  Disconnect/delete the current runtime.
2.  Connect to a fresh T4 GPU.
3.  Start running the notebook again from the top.

This avoids continuing with a runtime that may have lost its GPU.

* * *

# 3\. Installing the Required Libraries

The notebook installs two important libraries:

### BitsAndBytes

**BitsAndBytes** is used for **quantization**.

It allows models to be loaded using lower-precision representations, reducing their memory requirements.

### Accelerate

**Accelerate** helps run models efficiently on GPUs and provides supporting functionality for hardware acceleration.

Conceptually:

```
BitsAndBytes
→ Quantization

Accelerate
→ Efficient GPU execution
```

* * *

# 4\. Hugging Face Authentication

Before downloading certain models, you need to authenticate with Hugging Face.

The notebook provides a login mechanism where you use your Hugging Face access token.

The general process is:

```
Hugging Face account
       ↓
Access token
       ↓
Login from Colab
       ↓
Access models
```

* * *

# 5\. Access to Llama Models

Some Llama models require you to obtain access before downloading them.

The lesson specifically discusses access to:

-   Llama 3.1
-   Llama 3.2

If you already have access, you can proceed.

If you don't have access to Llama, the lesson suggests skipping the Llama section and working with the other models.

Llama 3.1 is larger and therefore takes longer to download and load.

* * *

# 6\. Models Used in the Exercise

The notebook works with several open-source models.

The lesson mentions:

-   **Llama 3.1**
-   **Llama 3.2**
-   **Phi** from Microsoft
-   **Gemma 3** from Google
-   **Qwen 3**
-   **DeepSeek distilled into Qwen**

The models have different sizes and characteristics.

One particularly important distinction is between:

### Instruct / Chat Models

These are designed around interactions such as:

```
User
 ↓
Assistant
 ↓
User
 ↓
Assistant
```

Examples include the instruct variants of the models being used.

### Reasoning Model

The DeepSeek model distilled into Qwen is used as an introduction to a **reasoning model**.

This provides an opportunity to run a reasoning model directly through the Hugging Face code.

* * *

# 7\. The Example Prompt

The same simple task is used across the models:

```
Tell a joke for a room of data scientists.
```

This makes it easier to compare how different models behave when given the same input.

* * *

# 8\. Quantization Configuration

```python
quant_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_use_double_quant=True,
    bnb_4bit_compute_dtype=torch.bfloat16,
    bnb_4bit_quant_type="nf4"
)
```

The notebook then creates a **BitsAndBytes configuration**.

This specifies how the model should be quantized when loaded.

The important setting is:

> **4-bit quantization**

Instead of keeping the model parameters at their original higher precision, they are represented using a much smaller number of bits.

Conceptually:

```
Original model
      ↓
Quantization configuration
      ↓
4-bit representation
      ↓
Reduced GPU memory usage
```

* * *

# 9\. NF4 Quantization

The configuration uses the **NF4** data type.

NF4 was introduced in the previous lesson.

The important idea is that the four bits are **not simply interpreted as an ordinary integer from 0 to 15**.

Instead, the bits are used to represent values from the floating-point parameter space in a way designed to work well with neural-network weights.

Conceptually:

```
4 bits
  ↓
NF4 representation
  ↓
Approximation of floating-point weights
  ↓
Much lower memory usage
```

* * *

# 10\. Creating the Tokenizer

The next step is to create the tokenizer for the selected model.

The general approach is:

Python

Run

```
tokenizer = AutoTokenizer.from_pretrained(...)
```

The tokenizer is loaded specifically for the chosen model.

This is important because, as covered previously:

> **Different models can have different tokenizers.**

* * *

# 11\. The Padding Token
```python
# Tokenizer

tokenizer = AutoTokenizer.from_pretrained(LLAMA)
tokenizer.pad_token = tokenizer.eos_token
inputs = tokenizer.apply_chat_template(messages, return_tensors="pt").to("cuda")
```

The notebook includes a line similar to:

Python

Run

```
tokenizer.pad_token = tokenizer.eos_token
```

This can initially look strange.

The tokenizer has a special token called:

**EOS — End Of Sentence**

Hugging Face also expects a **padding token** to be available in certain situations.

For some tokenizers, the padding token isn't already configured.

Therefore, the notebook sets:

```
Padding token
      ↓
End-of-sentence token
```

This is a common piece of setup code when working with certain Hugging Face models.

If it isn't configured, you may encounter an error later.

* * *

# 12\. Applying the Chat Template

The notebook then applies the model's **chat template**.

Conceptually:

```
Messages
   ↓
Chat template
   ↓
Formatted model input
   ↓
Token IDs
```

The chat template converts the conversational messages into the format expected by that particular model.

This is especially important for instruct/chat models because they have specific conventions for representing:

-   User messages
-   Assistant messages
-   Special tokens
-   Conversation boundaries

* * *

# 13\. Moving the Input to the GPU

The tokenized input needs to be converted into a format that PyTorch can work with.

The notebook uses:

Python

Run

```
return_tensors="pt"
```

`pt` refers to **PyTorch**.

This converts the tokenized data into a PyTorch tensor.

A tensor can be thought of, at this level, as a numerical matrix/data structure that PyTorch can operate on.

The input is then moved to the GPU using CUDA.

Conceptually:

```
Token IDs
   ↓
PyTorch tensor
   ↓
CUDA
   ↓
GPU
```

CUDA is NVIDIA's GPU computing technology.

* * *

# 14\. Why Move the Input to the GPU?

LLMs perform enormous amounts of mathematical computation.

GPUs are particularly effective at the large matrix operations involved in neural networks.

Therefore:

```
CPU
→ General-purpose computation

GPU
→ Highly parallel mathematical operations
→ Much faster for neural-network workloads
```

The tokenized input must therefore be placed on the GPU when the model is being executed there.

* * *

# 15\. Creating the Model

The tokenizer was created using `AutoTokenizer`.

The model is loaded similarly using:

Python

Run

```
AutoModelForCausalLM.from_pretrained(...)
```

The important part is:

> **CausalLM**

* * *

# 16\. What Is a Causal Language Model?

Hugging Face uses **Causal Language Model** or **CausalLM** for the type of LLM being used here.

A causal language model generates text by predicting tokens sequentially.

For example:

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
...
```

This is also commonly called an **autoregressive language model**.

So:

```
Causal LM
≈
Autoregressive LLM
```

The lesson uses **CausalLM** as the Hugging Face terminology for these generative language models.

* * *

# 17\. AutoModelForCausalLM

The model-loading code conceptually says:

```
AutoModelForCausalLM
        ↓
Find the appropriate model implementation
        ↓
Load the selected pretrained model
```

The model is loaded using the appropriate configuration for the chosen model.

The notebook also tells the model to use CUDA/GPU execution and passes in the quantization configuration.

* * *

# 18\. What Happens When the Model Is Loaded?

When the model-loading code executes, several things happen:

```
Hugging Face
     ↓
Download model files
     ↓
Load model
     ↓
Apply quantization
     ↓
Place model in GPU memory
```

The full-precision model files are downloaded, after which the model is represented in the lower-precision format used in memory.

This is why even a relatively small model can take some time to load.

* * *

# 19\. Model Size Matters

The lesson highlights the difference between model sizes.

For example:

```
Llama 3.1
→ Larger
→ More parameters
→ Longer download/load time

Llama 3.2
→ Smaller
→ Faster
```

Even a model with around **1 billion parameters** can take some time to download and load.

A larger model such as Llama 3.1 can take significantly longer.

* * *

# 20\. End-to-End Flow

The complete process being built in this notebook is:

```
Hugging Face Model
       ↓
Quantization Configuration
       ↓
Load Tokenizer
       ↓
Apply Chat Template
       ↓
Token IDs
       ↓
PyTorch Tensor
       ↓
Move to GPU
       ↓
Load CausalLM
       ↓
Download Model
       ↓
Quantize / Load into GPU memory
       ↓
Generate Output
```

* * *

# Key Takeaways

-   This lesson works directly with the **Hugging Face Transformers low-level API**.
-   The examples run on a **Google Colab T4 GPU**.
-   **BitsAndBytes** is used for model quantization.
-   **Accelerate** helps with efficient GPU execution.
-   Some Llama models require Hugging Face access approval.
-   Several open-source models are compared, including Llama, Phi, Gemma, Qwen, and a DeepSeek-distilled Qwen model.
-   The exercise includes both **instruct/chat models** and a **reasoning model**.
-   The example prompt is:

```
Tell a joke for a room of data scientists.
```

-   The model is configured for **4-bit quantization**.
-   **NF4** is used as the 4-bit representation.
-   The tokenizer is loaded using `AutoTokenizer.from_pretrained()`.
-   Some tokenizers require the padding token to be explicitly configured:

Python

Run

```
tokenizer.pad_token = tokenizer.eos_token
```

-   Chat messages are converted into model-specific input using the **chat template**.
-   `return_tensors="pt"` converts the tokenized input into a PyTorch tensor.
-   CUDA is used to move the input to the NVIDIA GPU.
-   `AutoModelForCausalLM` loads a generative causal language model.
-   **CausalLM** refers to the autoregressive approach where the model predicts tokens sequentially.
-   Loading a model involves downloading its files, applying the configured quantization, and placing the model into GPU memory.
-   Larger models require more time and resources to download and load.