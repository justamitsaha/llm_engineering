## 1\. Moving from Tokenizers to Models

Week 3 has progressed from:

```
Hugging Face Hub
        ↓
Hugging Face Libraries
        ↓
Pipelines
        ↓
Tokenizers
        ↓
Models
```

Day 4 focuses on the **Models** part of the Hugging Face Transformers library.

The goal is to:

-   Run actual open-source models using Python.
-   Compare five different open-source models.
-   Understand some of the underlying neural-network concepts.
-   Look at neural-network layers and dimensionality.
-   Introduce quantization.
-   Introduce streaming when working with open-source models.

The emphasis remains practical rather than deeply theoretical.

* * *

# 2\. Running Models Directly

With Hugging Face Transformers, we can move beyond the high-level Pipeline API and directly run the Python code that drives a deep neural network.

This provides significantly more control over the model.

```
Pipelines
→ Simple interface

Transformers Models
→ Direct interaction with the model
→ More control
→ More technical detail
```

The lesson will compare **five different open-source models** while using this process to develop intuition about how neural networks actually work.

* * *

# 3\. Deep Neural Networks Contain Huge Numbers of Parameters

A neural network contains a very large number of **parameters**.

Modern LLMs can contain **billions of parameters**.

These parameters are numbers that participate in the mathematical operations performed by the neural network.

A simplified view is:

```
Parameters
     ↓
Matrix calculations
     ↓
GPU computations
     ↓
Neural-network output
```

During inference, huge numbers of these values are multiplied and added together, often through large matrix operations performed on GPUs.

* * *

# 4\. Parameter Precision

The parameters of these models are typically represented using numerical formats such as:

-   **16-bit**
-   **32-bit**

A 16-bit number is represented using 16 binary digits.

A 32-bit number uses 32 binary digits.

More bits allow a number to represent a larger range and/or greater precision.

Conceptually:

```
32-bit
→ More possible numerical values
→ Higher precision
→ More memory

16-bit
→ Fewer possible values
→ Less memory
```

This leads to an important optimization technique:

> **Quantization**

* * *

# 5\. What Is Quantization?

**Quantization** is the process of reducing the numerical precision used to represent model parameters.

Instead of storing parameters using 16 or 32 bits, we can represent them using fewer bits.

For example:

```
32-bit
   ↓
16-bit
   ↓
8-bit
   ↓
4-bit
```

The fewer bits used, the less memory is required.

* * *

# 6\. The Dimmer-Switch Analogy

The lesson uses a **dimmer switch** analogy.

Imagine every model parameter as a dimmer switch controlling the brightness of a light.

With 32 bits, there are an enormous number of possible settings.

The brightness can therefore change almost continuously.

With fewer bits, there are fewer possible settings.

For example:

### 8-bit

An 8-bit value has:

```
2⁸ = 256
```

possible values.

### 4-bit

A 4-bit value has:

```
2⁴ = 16
```

possible values.

So reducing precision is like turning:

```
Smooth dimmer
      ↓
16 discrete brightness levels
```

The neural network's parameters become less precise.

* * *

# 7\. Why Quantize a Model?

The main reason is **memory efficiency**.

If each parameter requires fewer bits:

-   The model occupies less memory.
-   More of the model can fit into GPU memory.
-   Calculations can be simpler.
-   Larger models can potentially be run on hardware with limited memory.

For example, moving from 16-bit representation to 4-bit representation reduces the amount of storage required per parameter by a factor of four.

```
16 bits → 4 bits

4× less memory per parameter
```

* * *

# 8\. Does Quantization Destroy Model Quality?

At first, it might seem that reducing precision should have the same effect as simply removing parameters.

For example:

```
Original:
100% of parameters × high precision

Alternative:
25% of parameters × high precision
```

It might seem that both approaches throw away roughly the same amount of information.

But experimentally, this isn't what happens.

### Quantization

Reducing precision:

```
Model quality
↓
Small decrease
```

### Removing parameters

Removing a large proportion of parameters:

```
Model quality
↓↓↓↓
Much larger decrease
```

Therefore, quantization turns out to be a surprisingly efficient way of reducing memory requirements while preserving much of the model's performance.

* * *

# 9\. Why Does Quantization Work?

The lesson describes this as somewhat experimental and not completely understood.

One intuition is that the model may contain more parameters than are strictly necessary at full precision.

Reducing their precision can therefore compress the information into a smaller representation without destroying most of the useful information.

The important practical observation is:

> **Quantization works surprisingly well.**

The course treats it as somewhat of a "hack" or practical trick rather than going deeply into the mathematical theory behind why it works.

* * *

# 10\. Quantization Is Not Simply a 4-Bit Integer

A potential misunderstanding is:

> "If a model normally uses floating-point numbers and we reduce them to 4 bits, does that mean every parameter can only be an integer from 0 to 15?"

No.

A 4-bit quantized model does **not** simply replace every floating-point parameter with an ordinary 4-bit integer.

Instead, techniques are used to represent the original floating-point values using a smaller number of bits.

The lesson introduces a data type called:

> **NF4**

NF4 is a four-bit representation designed to represent floating-point values efficiently, with the assumption that the values are approximately normally distributed.

The details of how NF4 maps the values are more advanced, and the course will demonstrate it practically.

* * *

# 11\. NF4

The important conceptual difference is:

```
4-bit integer
→ 16 integer values

NF4
→ Uses 4 bits to represent information from
   the floating-point parameter space
```

The lesson emphasizes that it may seem surprising that this works effectively, but it does work well enough to be widely useful for quantized models.

* * *

# 12\. Quantization: Overall Picture

```
Full-precision model
        ↓
Large number of parameters
        ↓
16/32-bit representations
        ↓
High memory usage
        ↓
Quantization
        ↓
Lower-precision representations
        ↓
Much lower memory usage
        ↓
Only relatively small degradation in quality
```

This makes quantization particularly useful when trying to run large models on hardware with limited GPU memory.

* * *

# 13\. Looking Inside the Neural Network

The lesson will also briefly look inside the model.

The goal is **not** to study neural-network architecture in exhaustive theoretical detail.

Instead, the focus is on developing intuition around concepts such as:

-   Neural-network layers
-   Dimensionality
-   How information moves through the model

The approach remains practical:

> Understand enough of the internals to know what the model is doing, without getting buried in architecture diagrams and theory.

* * *

# 14\. Streaming

Another topic introduced is **streaming**.

When working with open-source models, streaming allows model output to be handled as it becomes available rather than necessarily waiting for the complete response.

This is particularly relevant when generating text interactively.

The lesson introduces streaming as an additional practical technique that will be explored in the lab.

* * *

# 15\. What You'll Do in the Lab

The lab will bring together the concepts introduced in this lesson.

You will:

1.  Work directly with Hugging Face Transformers models.
2.  Run open-source models using Python.
3.  Compare five different models.
4.  Explore some neural-network internals.
5.  Experiment with quantization.
6.  Look at lower-precision representations such as NF4.
7.  Explore streaming.

The exercises are performed in **Google Colab**.

* * *

# Key Takeaways

-   Day 4 moves from **tokenizers to actual Hugging Face models**.
-   The **Transformers** library allows you to run open-source models directly.
-   Modern LLMs contain billions of numerical **parameters**.
-   These parameters are typically represented using 16-bit or 32-bit numerical formats.
-   Higher precision requires more memory.
-   **Quantization** reduces the number of bits used to represent model parameters.
-   Common examples include moving from 16-bit to 8-bit or 4-bit representations.
-   A 4-bit value has only **16 possible binary values**, compared with 256 for 8-bit.
-   Quantization dramatically reduces memory requirements.
-   Reducing precision causes some loss in model quality, but typically much less than simply removing a large proportion of parameters.
-   Quantization is therefore an efficient practical technique for fitting large models into smaller amounts of memory.
-   **NF4** is a four-bit representation designed for representing floating-point model values efficiently.
-   The lesson also introduces neural-network **layers, dimensionality, and streaming**.
-   The emphasis is practical: understand enough of the internals to work effectively with open-source models.