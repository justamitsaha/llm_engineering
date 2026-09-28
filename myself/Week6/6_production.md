# Productionizing AI & The Price Is Right Capstone

## 1. The Complete Five-Step AI Strategy

The overall process for applying AI to a business problem is:

**1. Understand → 2. Prepare → 3. Select → 4. Customize/Apply → 5. Productionize**

The first four steps have already been covered:

* **Understand** the business requirements.
* **Prepare** candidate models, a baseline model, and a curated dataset.
* **Select** the model through experimentation and evaluation.
* **Customize/Apply** the model using techniques such as prompting, RAG, agents, or fine-tuning.

The final step is **productionization**.

---

# 2. Step 5 — Productionize

Productionization means taking the AI system you've built and **putting it into a real production environment**.

This is also commonly associated with **MLOps** and is a discipline of its own.

### Major production concerns

You need to consider:

* **Hosting and deployment architecture**
* **Production cloud environment**

  * AWS
  * GCP
  * Azure
* **Scalability**
* **Monitoring**
* **Security**
* **Compliance**
* **Observability**
* **Evaluation in production**

For agentic systems, observability becomes particularly important because you may want to monitor what the different LLMs are doing and saying to each other.

---

# 3. Evaluation Doesn't Stop After Development

A very important point is that **evaluation must continue after deployment**.

The process is not:

**Build → Evaluate → Deploy → Done**

Instead:

**Build → Evaluate → Deploy → Continuously Evaluate → Improve → Retrain → Evaluate again**

### Why?

Because of **model drift**.

### Model Drift

Model drift refers to the tendency for a system's performance to decline over time when **real-world data starts to differ from the data used for training**.

Therefore, production systems need:

* Continuous evaluation
* Performance monitoring
* Retraining when necessary
* Repeated measurement

### Key principle

> **Evals are continuous, not something you do only during development.**

---

# 4. The Five-Step Process

The complete process is:

| Step                     | Purpose                                                  |
| ------------------------ | -------------------------------------------------------- |
| **1. Understand**        | Understand the business problem and requirements         |
| **2. Prepare**           | Prepare data, baseline, and candidate models             |
| **3. Select**            | Experiment and choose the model for the task             |
| **4. Customize / Apply** | Apply techniques and optimize against evaluation metrics |
| **5. Productionize**     | Deploy, monitor, evaluate, and continuously improve      |

This five-step framework should be treated as a **discipline** whenever solving business problems with AI.

---

# 5. The Price Is Right — Business Requirements

The capstone project is called **“The Price Is Right.”**

### Problem

Given:

**Product description → Predict the product's actual price**

### Input

A description of a product.

### Output

The predicted price of that product.

---

# 6. Future Agentic System

The eventual goal is larger than simply predicting prices.

The price prediction capability will become part of an **agentic platform** that can:

1. Search the internet for products.
2. Find potential bargains.
3. Estimate the product's real value.
4. Determine whether an apparent bargain is actually an opportunity.
5. Alert the user when a potentially valuable deal is found.

Conceptually:

**Internet → Find products → Predict real price → Compare with asking price → Identify opportunities → Alert user**

---

# 7. Evaluation Metric

The project uses a very simple evaluation metric:

### Absolute Price Difference

**Absolute error = |Predicted Price − True Price|**

For example:

If:

* True price = $100
* Predicted price = $90

Then:

**Absolute error = $10**

This metric is useful because it is:

* Easy to understand
* Directly related to the prediction task
* Both a **model metric** and a **business outcome metric**

The project therefore focuses heavily on this measurement.

---

# 8. Week 6 — Order of Activities

The current week focuses on:

* Data curation
* Evaluation
* Frontier models

### Day 1

**Data curation**

Already completed.

### Day 2

**Pre-processing**

The instructor will:

* Use an LLM to rewrite product descriptions.
* Make the descriptions better suited for training.
* Use **batch mode** for LLM inference.

Batch mode is presented as a more advanced/professional technique worth learning.

### Day 3

**Traditional Machine Learning**

The course moves back to traditional ML and builds models as a progression toward more advanced approaches.

### Day 4

**Neural Networks + Frontier Models**

The project progresses from traditional ML into:

* Neural networks
* Frontier models

### Day 5

**Fine-tuning a Frontier Model**

A frontier model will finally be fine-tuned through an API.

The instructor notes that this is not as hands-on as fine-tuning an open-source model yourself, but it demonstrates the process and outcome.

---

# 9. Week 7 — Fine-Tuning an Open-Source Model

The following week involves much more hands-on fine-tuning.

The project will start with an existing **pre-trained open-source model**, rather than creating a model completely from scratch.

The transcript mentions:

**Llama 4 from Meta**

as the starting model.

### Important terminology

When people say:

> “I fine-tuned my own model.”

they generally **do not mean** they trained an LLM from zero.

Instead:

**Existing pre-trained model → Add your own data → Fine-tune → Specialized model**

Training a model completely from scratch would require enormous resources and is generally not commercially practical for this type of project.

---

# 10. Training From Scratch vs Fine-Tuning

### Training from scratch

Would involve:

* Starting with essentially no pre-trained model
* Training on tokens/data
* Very large computational requirements

The transcript says this can still be useful for **toy LLM projects**, such as creating a small model that writes children's stories from a limited corpus.

### Fine-tuning

Instead:

**Start with a pre-trained model → Further train it using your own data**

This is the typical meaning of “building my own model” in an industry context.

---

# 11. Week 8 — RAG & Agentic AI

Week 8 returns to inference-time techniques and more advanced systems.

Planned topics include:

* RAG
* Agentic AI
* Building an agentic platform

This brings together many of the concepts covered throughout the course.

---

# 12. Three Ways to Follow the Capstone

The instructor provides three different levels of participation.

### Option 1 — Get the Gist

Focus on:

* Listening to the explanations
* Understanding the decisions
* Understanding the overall approach

No need to reproduce the entire implementation.

This is useful if you want to understand the concepts and potentially apply them to a project later.

---

### Option 2 — Lite Version

Run the project while avoiding the full workload.

Use:

* The **light dataset**
* The instructor's curated data

You don't have to reproduce all the data curation and preprocessing.

The light dataset can still be used for experimenting with preprocessing if desired.

---

### Option 3 — Go Big

Follow the complete project:

* Use the full dataset
* Use the approximately **800,000-item dataset**
* Build your own model
* Run your own experiments
* Try to improve on the instructor's results

This option is explicitly framed as a challenge.

---

# 13. The Core Challenge: Experimentation

The instructor emphasizes that there is **no magic trick** for getting a better result.

Improvement comes from:

**Experiment → Measure → Learn → Try another experiment → Repeat**

The goal for students doing the full project is to think independently and try different experiments rather than simply reproducing the instructor's exact approach.

The instructor's point is that discovering better performance is primarily about **systematic experimentation and iteration**, rather than knowing one secret hyperparameter or technique.

---

# 14. Overall Course Progression

The larger course progression can be summarized as:

**Week 6**
Data → Preprocessing → Traditional ML → Neural Networks → Frontier Models → Fine-tuning via API

↓

**Week 7**
Hands-on fine-tuning of an existing open-source model

↓

**Week 8**
RAG → Agentic AI → Agentic platform

↓

**Final Goal**
Combine training-time and inference-time techniques into a practical AI system.

---

# Key Takeaways

* AI projects should follow a **five-step process**:
  **Understand → Prepare → Select → Apply → Productionize**
* **Productionization** involves deployment, hosting, scalability, security, compliance, monitoring, and observability.
* **Evaluation continues after deployment**; it doesn't stop when the model goes live.
* **Model drift** occurs when real-world data changes relative to the training data and can cause performance to decline.
* The Price Is Right predicts **product price from product description**.
* Its primary evaluation metric is **absolute price difference**.
* The eventual goal is to use price prediction inside an **agentic bargain-finding system**.
* Week 6 progresses from **data preprocessing → traditional ML → neural networks → frontier models → fine-tuning**.
* Week 7 focuses on **fine-tuning an existing open-source model**.
* “Fine-tuning my own model” generally means starting with a **pre-trained model** and training it further with your own data.
* Week 8 returns to **RAG and agentic AI**.
* Students can follow the project at three levels: **understand the concepts, use the light dataset, or complete the full project**.
* The central philosophy is **experimentation**: there is no shortcut to R&D.
