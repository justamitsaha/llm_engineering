## Data: The Foundation of AI

### 1. Data Is the New Electricity

* Andrew Ng famously described **AI as the new electricity**.
* The lecture presents a related idea: **data is what powers AI**.
* When building an AI system or startup, one of the first questions should be:

  > **Where will the data come from?**

### Main sources of data

1. **Proprietary data** — your own unique business data.
2. **Kaggle** — a major data-science community with many datasets.
3. **Hugging Face Datasets** — a major source of datasets for the LLM community.
4. **Synthetic data** — data generated artificially, often using AI.
5. **Specialized data companies** — organizations that curate or generate data, such as Scale.

---

# 2. Dataset for "The Price Is Right"

The project uses the **Amazon Reviews 2023** dataset available through Hugging Face.

It contains:

* Amazon product information
* Product descriptions
* Product prices
* Reviews

For this project, the focus is primarily on:

> **Product description → Product price**

The reviews are not required for the main project.

The dataset is extremely large and is based on Amazon data collected over multiple years.

---

# 3. Data Curation Is Extremely Important

A lot of attention in AI is placed on glamorous topics such as:

* Model training
* Hyperparameter optimization
* Fine-tuning
* Neural networks

However, **data curation can have an even bigger effect on model quality**.

Data curation involves:

* Investigating the dataset.
* Checking its quality.
* Parsing the data.
* Understanding its structure.
* Visualizing it.
* Identifying problems.
* Selecting an appropriate distribution of examples.
* Preparing the final dataset.
* Uploading the curated dataset to the Hugging Face Hub.

### Key idea

> **A high-quality dataset can have a major impact on the quality of model training.**

Therefore, data preparation should not be treated as an unimportant preliminary step.

---

# 4. Evaluating Model Performance

Another major theme is **evaluation**.

There are two broad types of metrics:

### A. Model-Centric / Technical Metrics

These measure how well the model itself is performing.

They are closely connected to the model's training process.

A common example for LLMs is:

> **Cross-entropy loss**

Another common traditional ML metric is:

> **Mean Squared Error (MSE)**

MSE measures the squared difference between the model's prediction and the actual value.

---

### B. Business-Centric / Outcome Metrics

These measure whether the model is achieving the **actual business objective**.

Examples include:

* User thumbs-up/thumbs-down feedback
* Revenue
* Customer satisfaction

The challenge is that business metrics can sometimes be difficult to connect directly to individual model changes.

Ideally, you want a metric that is:

> **Close to the model's behavior AND close to the final business objective.**

---

# 5. The Project's Evaluation Metric

For the Price Is Right project, there is a particularly straightforward metric.

The model predicts:

> **Predicted price**

We already know:

> **Actual price**

So we can calculate:

> **Absolute price error = |Predicted Price − Actual Price|**

For example:

If the actual price is $500 and the model predicts $600:

> Error = $100

This metric is easy for both technical and business audiences to understand.

Instead of saying:

> "The MSE is 10,000."

We can say:

> **"The model is, on average, $100 away from the actual price."**

That makes the performance much more tangible.

---

# 6. Three Data Splits

The dataset is divided into **three separate pools**:

### 1. Training Data

This is the **largest portion** of the dataset.

It is used to train the model.

The training data contains:

> **Inputs + expected outputs**

For this project:

> Product description → Product price

The model uses this data to adjust its internal parameters.

---

### 2. Validation Data

Also called:

* **Validation data**
* **Val data**
* **Eval data**

This is **held-out data**, meaning it is not used to train the model.

Its purpose is to check:

> **How well does the model perform on unseen data?**

During training, the validation set is repeatedly used to determine whether the model is getting better or worse.

This helps select the best version of the model.

---

### 3. Test Data

The test set is another portion of **unseen, held-out data**.

However, unlike validation data, it is kept in reserve until the **very end**.

Why?

Because the validation data has already been used repeatedly during training to choose the best model.

If we then evaluate that same data again, the evaluation is no longer completely independent.

Therefore:

> **Test data provides a final evaluation of how well the selected model generalizes to unseen data.**

---

# 7. Training vs Validation vs Test

A simple way to remember the three:

| Dataset        | Purpose               | Used When                     |
| -------------- | --------------------- | ----------------------------- |
| **Training**   | Teach the model       | During training               |
| **Validation** | Select/tune the model | Repeatedly during development |
| **Test**       | Final evaluation      | At the very end               |

### Conceptual flow

**Training data**
↓
Train the model
↓
**Validation data**
↓
Choose/improve the model
↓
**Test data**
↓
Final measurement of generalization

---

# 8. Dataset Proportions

In traditional machine learning, a common split was approximately:

> **80% Training / 10% Validation / 10% Test**

With LLMs, the split is often much more heavily weighted toward training because LLMs require enormous amounts of training data.

Therefore:

* Training data can be extremely large.
* Validation data can be relatively small.
* Test data can also be relatively small.

The validation and test sets still need to be large enough to produce **stable evaluation metrics**.

---

# Key Takeaways

### Data

> **Data is a fundamental resource powering AI systems.**

### Data sources

> Proprietary data, Kaggle, Hugging Face, synthetic data, and specialized data companies are important sources.

### Data curation

> **Curating a high-quality dataset can have a major impact on model performance**, sometimes more than changing model-training settings.

### Evaluation

> Use metrics that measure model performance while remaining meaningful for the actual business objective.

### Price Is Right metric

> **Absolute difference between predicted and actual price** gives a simple, tangible measure of performance.

### Three datasets

> **Training → Validation → Test**

* **Training:** learn from the data.
* **Validation:** repeatedly evaluate and select the model during development.
* **Test:** perform the final unbiased evaluation on unseen data.

### Overall workflow

**Raw Amazon Data → Investigate → Parse → Visualize → Curate → Split → Train → Validate → Test**

The next practical step is to **curate the Amazon product-price dataset** for the Price Is Right project.
