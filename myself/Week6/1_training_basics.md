## AI Training and Generalization

### 1. Training in Machine Learning

* **Training** is at the heart of AI and machine learning.
* A model makes predictions using internal settings called **parameters** or **weights**.
* Training means adjusting these parameters using examples of **inputs and expected outputs**.
* The goal is to find parameter settings that allow the model to recognize patterns in the training data.

**Basic idea:**

> Input → Model → Output
> Training adjusts the model's parameters so that the output matches the expected output.

---

### 2. Training Is NOT Just Memorization

* The goal of training is **not** simply to memorize the examples provided during training.
* If a model sees 10 input-output examples, it should obviously perform well when given those same 10 examples.
* What really matters is what happens when the model receives an **input it has never seen before**.
* The model should use the patterns it learned from the training examples to produce a sensible output for the new input.

This unseen information is called **unseen data**.

---

### 3. Generalization — The Most Important Concept

* **Generalization** is the ability of a model to make accurate or sensible predictions on **new data that it did not see during training**.
* A model has learned successfully when it can recognize the underlying patterns rather than simply memorize its training examples.

**In simple terms:**

> Training = Learn patterns from examples
> Generalization = Apply those patterns to new examples

The overall goal of AI training is therefore to create models that **generalize well**.

---

### 4. Why LLMs Like GPT Are Remarkable

* The impressive capability of LLMs such as **GPT** is not merely that they can reproduce things similar to their training data.
* Their real strength is that they can combine and generalize patterns in ways that may not have appeared explicitly in their training examples.
* For example, GPT can be asked to:

  * Write a poem.
  * Make it use **rhyming couplets**.
  * Require every other sentence to begin with the letter **A**.
* Even if that exact combination was never present in its training data, the model can often produce a coherent result.

This demonstrates **generalization**.

---

### 5. Generalization in LLMs

* For an LLM, training involves seeing huge numbers of examples of **inputs and their following tokens**.
* By seeing enormous numbers of these patterns, the model learns relationships between contexts and likely next tokens.
* It can then use those learned patterns to generate outputs for new combinations of inputs.

### Key Takeaway

> **The purpose of AI training is not simply to memorize training examples. It is to adjust the model's parameters so that the model learns patterns and can generalize those patterns to unseen data.**

**Generalization = the ability to perform well on new data that is different from, but related to, the data seen during training.**

This ability to generalize is one of the central ideas behind modern AI and the capabilities of LLMs such as GPT.

## Fine-Tuning, Transfer Learning & The Price Is Right

### 1. Traditional ML vs. LLM Training

* Traditional machine-learning models often had relatively few parameters—around **20 to 200** in the examples discussed.
* Training involved using a dataset to adjust those parameters so the model could make useful predictions.
* **LLMs are dramatically larger**, potentially containing **billions of parameters**.
* Training large LLMs can therefore cost **millions or even hundreds of millions of dollars**.
* Because training a large model from scratch is expensive, we can take advantage of **transfer learning**.

---

### 2. Transfer Learning

**Transfer learning** means taking a model that has already been trained and using it as the starting point for another task.

* A previously trained model is called a **pre-trained model**.
* Instead of training a huge model from scratch, we:

  1. Start with the pre-trained model.
  2. Give it additional, task-specific data.
  3. Train it further for a particular purpose.
* The idea is to retain the model's existing knowledge while making it better at a specific task.

---

### 3. Fine-Tuning

**Fine-tuning** is the process of using transfer learning to train a pre-trained model further for a particular task.

> **Pre-trained model + task-specific training → Fine-tuned model**

The course will:

* Fine-tune a **frontier model through APIs**.
* Later fine-tune an **open-source model directly**.

---

# The Capstone Project: "The Price Is Right"

The main project for the next several weeks is called **"The Price Is Right."**

### Goal

Build a system that can:

> **Take a description of a product and predict how much that product should cost.**

Examples include:

* TVs
* Computer monitors
* Computers
* Car parts
* Washing machines
* Other household products

Eventually, this prediction system will become part of a larger **agentic system**.

### Final Goal

The eventual agent will be able to:

1. Search the internet for products and bargains.
2. Determine what a product is really worth.
3. Compare that value with the selling price.
4. Decide whether a deal appears to be a genuine bargain.
5. Potentially perform autonomous actions based on that assessment.

---

# 4. Why Price Prediction Is a Regression Problem

Predicting the price of a product is a classic **regression problem**.

* Regression means predicting a **number** based on various factors.
* Traditional machine learning is generally well suited to regression.

For example, the price of a TV might depend on features such as:

* Screen size
* Product type
* Quality
* Whether it is a smart TV
* How the product is positioned
* Whether it is presented as a luxury product

These characteristics are called **features** in traditional machine learning.

---

# 5. Why Use LLMs for Price Prediction?

At first, price prediction might seem like something traditional ML should handle better than LLMs.

However, product descriptions contain a lot of **language-based nuance**.

An LLM can understand more than simple numerical features.

For example, it can potentially understand that:

> A 60-inch TV + premium quality + smart features + luxury positioning

may imply a different price than simply:

> A 60-inch TV.

LLMs already contain significant knowledge about language, products, and their descriptions.

The course will therefore compare several approaches.

---

# 6. Comparing Different Models

The project will evaluate:

1. **Traditional machine learning**
2. **Neural networks**
3. **Closed-source frontier LLMs**
4. **Open-source LLMs**
5. **Fine-tuned models**

The key evaluation question is:

> **How accurately can the model predict the price of a product using only its description?**

An advantage of this project is that the evaluation is very clear.

Because the actual price is known, we can directly measure how accurate the predictions are.

---

# 7. Course Plan: Week 6

The current week focuses on **data curation, evaluation, and frontier LLMs**.

### Day 1 — Data Curation

* Clean and examine the data.
* Work with the dataset.
* Explore and visualize the data.

### Day 2 — Data Pre-processing

* Process the data for use by the models.
* Use LLMs in an important way to rewrite/process data so that it works better for the model.

### Day 3 — Traditional Machine Learning

* Step back from LLMs.
* Build a **traditional ML baseline**.
* Establish what can be achieved without using LLMs.

### Day 4 — Neural Networks & Frontier LLMs

* Build a simple **artificial neural network using PyTorch**.
* Move from neural networks to **frontier LLMs**.
* Compare their performance.

### End of Week — Fine-Tuning

* Fine-tune a frontier LLM.
* The instructor warns that this part may produce disappointing results, but later weeks will build on it.

---

# 8. Weeks 7 and 8

### Week 7

Focus on **fine-tuning an open-source model**.

### Week 8

Focus on:

* **Deploying the model**
* Using a **serverless AI platform**
* Bringing back **RAG**
* Building the larger product-price agent

The final project combines the different techniques learned throughout the course.

---

# 9. Three Ways to Follow the Course

The instructor provides three different levels of participation.

### Option 1 — Focus on Intuition

For learners who mainly want to understand the concepts:

* Watch the lectures.
* Don't necessarily complete every lab.
* Focus on understanding the terminology and general ideas.
* Run the final result so you can see what happens.

**Goal:** Understand **what is happening and why**, without needing to implement everything yourself.

---

### Option 2 — Lite Version

For learners who want to follow along and implement things without spending much money or time:

* Use the smaller **20,000-example dataset** instead of the full 800,000-example dataset.
* Follow the data-processing steps.
* Use the instructor's already-prepared final curated/processed data where appropriate.
* This avoids downloading and processing several gigabytes of data.

Estimated cost:

> **Free to around $5**

---

### Option 3 — Full Version

For learners who want the complete experience:

* Use the full **800,000-example dataset**.
* Perform the complete data curation.
* Perform the complete training.
* Run the full experiments.

This can involve:

* Significant computational work.
* Several days for some processes to finish.
* More expense.

The stated estimate is approximately:

> **$5–$35**

The instructor notes that the largest potential expense is preprocessing the full dataset, but learners can avoid much of that cost by using the already-processed data provided.

---

# Key Takeaways

### Training large LLMs

> LLMs have billions of parameters, making training from scratch extremely expensive.

### Transfer Learning

> Start with an already-trained model instead of training from scratch.

### Fine-Tuning

> Further train a pre-trained model using task-specific data.

### Price Prediction

> The capstone project uses product descriptions to predict product prices.

### Regression

> Price prediction is fundamentally a **regression problem** because the output is a numerical value.

### Why LLMs?

> LLMs can understand the nuanced meaning contained in product descriptions, potentially giving them an advantage when language contains important information about price.

### Evaluation

> The project's major advantage is that prediction accuracy can be measured directly because the actual product prices are known.

### Overall Journey

**Data → Data Curation → Pre-processing → Traditional ML → Neural Networks → Frontier LLMs → Fine-Tuning → Open-Source Models → Deployment → RAG + Agentic System**

The central project tying all of this together is **"The Price Is Right."**
