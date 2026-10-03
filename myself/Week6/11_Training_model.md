# Week 6, Day 4 — Neural Networks, Training, and Hyperparameter Optimization

## 1. Where We Are in the Course

By this point, the course has covered:

* Generating **code and text** using open- and closed-source models.
* Building and evaluating **RAG solutions**.
* Following a **five-step AI problem-solving process**.
* Creating and curating datasets.
* Building **traditional ML baseline models**:

  * Linear Regression
  * Random Forest
  * XGBoost
* Visualizing model performance using the **ŷ vs. y chart**.

### Today's progression

The focus now moves from traditional ML to:

**Traditional ML → Neural Networks → Frontier Models → Fine-Tuning → RAG + Agentic AI**

---

# 2. What Happens During Week 6?

The overall focus of the week is:

1. **Curate data**
2. **Preprocess data**
3. **Build and evaluate baseline models**
4. **Train a neural network**
5. **Experiment with frontier models**

### Previous days

**Day 1 — Data Curation**

* Collected product data from Amazon-related datasets on Hugging Face.
* Examined prices and product descriptions.
* Ensured the dataset contained useful product/price information.

**Day 2 — Preprocessing + Traditional ML**

* Used an LLM to rewrite product descriptions into shorter, higher-signal descriptions.
* Used GPT-OSS to perform this preprocessing.
* Built increasingly sophisticated baselines:

  * Random prediction
  * Average/straight-line prediction
  * Linear Regression
  * NLP-based models
  * Random Forest
  * XGBoost

**Day 4 — Today**

* Build a **vanilla neural network**.
* Train it from scratch.
* Evaluate how it performs.
* Then move to **frontier models**.
* Compare several frontier models using the same evaluation framework.

---

# 3. Neural Networks and Frontier Models

The next step is a **vanilla neural network**.

The goal is not simply to build it but to actually **train it** on the product-price prediction problem.

After that, the course moves to **frontier models**.

### Frontier models

Frontier LLMs are also neural networks, but they use much more sophisticated architectures.

They typically involve:

* **Self-attention**
* **Transformer architecture**
* Very large numbers of parameters

So the progression is:

**Traditional ML → Basic Neural Network → Frontier Neural Network**

The lecture notes that frontier models will eventually lead into:

* Fine-tuning
* Open-source model fine-tuning
* RAG
* Agentic AI

---

# 4. What Is Training?

A machine-learning model contains **parameters**.

Parameters determine how the model converts its inputs into predictions.

### Traditional ML

Traditional ML models might have roughly:

**2–20 parameters**

### Deep Learning / LLMs

Modern neural networks can have:

* Millions of parameters
* Billions of parameters
* Sometimes trillions of parameters

During training, these parameters are repeatedly adjusted so the model becomes better at predicting outputs.

### But training is not simply memorization

The goal is not:

> Memorize the training examples.

The goal is:

> Learn patterns that allow the model to perform well on new, unseen inputs.

This ability is called **generalization**.

---

# 5. Generalization vs. Overfitting

### Generalization

A well-trained model should perform well not only on its training examples but also on **new data**.

Example:

If a model learns relationships between:

**Product description → Product price**

it should be able to estimate the price of a product it has never encountered before.

### Overfitting

If a model learns the training examples too specifically, it may perform well on training data but poorly on new data.

This is **overfitting**.

### The basic idea

**Good training:**

`Learn general patterns → Perform well on unseen data`

**Overfitting:**

`Memorize specific training patterns → Perform poorly on unseen data`

---

# 6. The Four Steps of Neural Network Training

Training repeatedly performs four major steps.

The dataset is normally divided into small groups called **batches**.

For each batch, the model goes through:

### 1. Forward Pass

The model receives inputs and produces a prediction.

For example:

**Product description → Neural network → Predicted price (ŷ)**

This calculation is called the **forward pass**.

---

### 2. Loss Calculation

Because this is training data, we know the actual answer:

* `ŷ` = predicted value
* `y` = actual/ground-truth value

The model compares the prediction with the actual answer.

The difference is represented by the **loss**.

Conceptually:

`Prediction → Compare with ground truth → Calculate loss`

The loss tells the model how badly it performed.

---

### 3. Backward Pass

Now the model works backward through the network.

The objective is to determine:

> Which parameters contributed to the error, and in which direction should they change?

This involves calculating **gradients**.

Gradients tell us how sensitive the loss is to changes in each parameter.

This is where calculus is involved.

---

### 4. Optimization

The model then adjusts its parameters slightly in a direction that should reduce the loss.

One simple approach is:

**Stochastic Gradient Descent (SGD)**

The basic idea is:

`Calculate loss → Calculate gradients → Adjust parameters → Try again`

The parameters are gradually tweaked so that the model performs better.

---

# 7. The Complete Training Loop

The four steps can be visualized as:

**Input batch**
↓
**Forward Pass**
↓
**Prediction (ŷ)**
↓
**Loss Calculation**
↓
**Loss**
↓
**Backward Pass / Gradients**
↓
**Optimization**
↓
**Update Parameters**
↓
**Next Batch**
↓
**Repeat many times**

The important point is that these four operations happen repeatedly.

---

# 8. What Are Batches?

Instead of processing the entire dataset at once, training usually divides the data into smaller groups called **batches**.

For example:

`Dataset → Batch 1 → Batch 2 → Batch 3 → ...`

The model performs the four training steps on each batch.

### Batch size

The number of examples contained in each batch is a **hyperparameter**.

For example:

* Batch size = 16
* Batch size = 32
* Batch size = 64

The batch size itself is **not a model parameter**.

---

# 9. Parameters vs. Hyperparameters

This is an important distinction.

### Parameters

Parameters belong to the model and are **learned/adjusted during training**.

Examples include:

* Weights
* Other internal values that determine predictions

### Hyperparameters

Hyperparameters are settings that control the training process.

They are **not learned in the same way as model parameters**.

Examples mentioned in the lecture:

* Batch size
* Number of epochs
* Learning rate

### Simple comparison

| Model Parameters                  | Hyperparameters                        |
| --------------------------------- | -------------------------------------- |
| Learned during training           | Set by the practitioner                |
| Directly adjusted by optimization | Control how training happens           |
| Weights are an example            | Batch size is an example               |
| Can number in millions/billions   | Usually a much smaller set of settings |

---

# 10. What Is an Epoch?

An **epoch** represents one complete pass through the entire training dataset.

For example:

If the training dataset contains 800,000 examples:

**1 epoch = model has gone through all 800,000 examples once.**

Training may involve multiple epochs:

`Dataset → Epoch 1 → Epoch 2 → Epoch 3 → ...`

The lecture mentions that models may sometimes be trained for **10 or 20 epochs**, although the appropriate number depends on the problem.

---

# 11. What Is the Learning Rate?

The **learning rate** controls how large a parameter update is during optimization.

Think of optimization as taking steps toward a better solution.

### Small learning rate

Very small parameter changes:

`Step → Step → Step → Step`

This can be slow but allows finer adjustments.

### Large learning rate

Larger changes:

`BIG STEP → BIG STEP → BIG STEP`

This can move faster but may overshoot useful solutions.

The lecture also mentions techniques where training starts with larger steps and gradually reduces them, allowing increasingly fine adjustments.

---

# 12. Hyperparameter Optimization

Choosing good hyperparameters is called **hyperparameter optimization**.

The lecture mentions experimenting with settings such as:

* Batch size
* Epochs
* Learning rate

The objective is to find a combination that produces better model performance.

### The reality of hyperparameter optimization

Despite the sophisticated name, the fundamental process is largely:

**Trial → Evaluate → Change settings → Trial again**

In other words:

> **Trial and error.**

There can be sophisticated techniques for searching the space of possibilities, but experimentation remains central.

---

# 13. Training vs. Hyperparameter Optimization

These two concepts should not be confused.

### Training

Adjusts the **model parameters**.

Example:

`Weights → Updated through forward/loss/backward/optimization`

### Hyperparameter optimization

Experiments with **training settings**.

Example:

`Learning rate = 0.001 vs. 0.01`

`Batch size = 32 vs. 64`

`Epochs = 10 vs. 20`

So:

**Training → learns model parameters**

**Hyperparameter optimization → searches for good training settings**

---

# 14. Practical Learning Philosophy

The instructor emphasizes that this course is primarily **practical**.

Students are not expected to understand all the mathematical details immediately.

The approach is:

**Do → Experiment → Observe → Learn the theory gradually**

The neural-network training exercise provides a practical first exposure to concepts such as:

* Forward pass
* Loss
* Backward pass
* Gradients
* Optimization
* Parameters
* Hyperparameters
* Epochs
* Learning rate

More detailed theory will be covered later.

---

# 15. Overall Course Progression

The lecture's larger roadmap is:

### Completed / Current

**Data Curation**
↓
**Preprocessing**
↓
**Traditional ML Baselines**
↓
**Neural Networks**
↓
**Frontier Models**

### Coming Next

**Fine-tuning frontier models**
↓
**Fine-tuning open-source models**
↓
**RAG**
↓
**Agentic AI**
↓
**Final integrated project**

---

# Key Takeaways

1. **Training adjusts model parameters** so the model becomes better at making predictions.
2. The goal of training is **generalization**, not memorization.
3. **Overfitting** occurs when a model becomes too specialized to its training data.
4. Neural-network training consists of four repeating steps:

   * **Forward pass**
   * **Loss calculation**
   * **Backward pass**
   * **Optimization**
5. **Gradients** indicate how changing parameters affects the loss.
6. **Stochastic Gradient Descent** is one method for updating parameters to reduce loss.
7. Data is usually processed in **batches**.
8. **Parameters** are learned by the model; **hyperparameters** control the training process.
9. Important hyperparameters include:

   * Batch size
   * Epochs
   * Learning rate
10. **Hyperparameter optimization** is essentially systematic experimentation/trial and error to find effective settings.
11. The course is progressing from **traditional ML → neural networks → frontier models → fine-tuning → RAG → agentic AI**.

### Core Mental Model

**Training a neural network:**

`Batch of data`
→ **Forward pass**
→ **Prediction**
→ **Loss**
→ **Backward pass / gradients**
→ **Optimization**
→ **Update parameters**
→ **Repeat**

And the ultimate goal is:

**Learn useful patterns → Generalize → Perform well on unseen data**
