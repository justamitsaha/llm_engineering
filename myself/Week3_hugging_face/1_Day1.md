# 1\. What Is Hugging Face?

Hugging Face started as a New York City-based startup and eventually became one of the central platforms in the open-source AI ecosystem.

It is particularly associated with:

-   Open-source Transformers
-   Open-source AI models
-   Datasets
-   AI applications

There are two sides of Hugging Face. This section focuses on the **Hugging Face platform/website**, also known as the **Hugging Face Hub**.

A useful mental model is:

> **Hugging Face is somewhat like GitHub for the AI/data-science ecosystem.**

* * *

# 2\. The Three Main Parts of the Hugging Face Platform

## 2.1 Models

Hugging Face hosts a huge collection of open-source models.

Models can be used directly through Hugging Face or downloaded and used locally.

You can search models based on the task you want to perform, such as:

-   Text generation
-   Text-to-image
-   Text-to-video
-   Image generation
-   Other AI tasks

### Useful filters

Models can be filtered by:

-   Task
-   Framework
-   Language
-   License
-   Number of parameters
-   Other model characteristics

You can also sort models by:

-   Trending
-   Likes
-   Downloads
-   Recently updated

**Most likes and most downloads** are presented as useful indicators when deciding which models to investigate.

### Understanding model names

A model name generally contains:

```
provider/account → model name
```

For example, the first part identifies the provider or Hugging Face account, followed by the actual model name.

* * *

# 3\. Datasets

The second major part of Hugging Face is its collection of **datasets**.

These datasets can be shared and used by other people for:

-   Training models
-   Experimentation
-   Research
-   AI projects

The lesson compares Hugging Face with **Kaggle**.

Kaggle remains an important source of datasets, but Hugging Face has become particularly prominent for datasets related to:

-   Generative AI
-   Transformers
-   Machine learning models

### Finding datasets

Datasets can be filtered by:

-   Type
-   Size
-   Name
-   Subject/category

For example, searching for financial datasets can produce datasets related to:

-   Stock prices
-   Stock-market tweets
-   Sentiment
-   Financial information

Each dataset typically has a **dataset card** containing information about what the dataset represents and examples of the data.

* * *

# 4\. Spaces

The third major component is **Spaces**.

Spaces are essentially **hosted applications that people can interact with**.

They allow you to take an AI application and make it available online for others.

Spaces can host:

-   Gradio applications
-   Streamlit applications
-   Other applications running inside Docker containers

### Gradio + Spaces

Earlier in the course, Gradio could be temporarily hosted using:

Python

Run

```
share=True
```

However, this approach relies on temporary HTTP tunneling and can cause problems on some corporate networks.

A more robust approach is to deploy the Gradio application as a **Hugging Face Space**.

The deployment can be initiated with:

Bash

```
gradio deploy
```

This allows the Gradio application to be pushed to Hugging Face and hosted as a Space.

* * *

# 5\. How to Navigate Hugging Face

When visiting the Hugging Face platform, the three primary sections are:

```
Hugging Face Hub
│
├── Models
├── Datasets
└── Spaces
```

### Models

Use when you want to find an existing AI model.

### Datasets

Use when you need data for training, experimentation, or analysis.

### Spaces

Use when you want to run or share an AI application.

* * *

# 6\. Practical Model-Selection Workflow

Because Hugging Face contains an enormous number of models, you should **not simply browse randomly**.

A better workflow is:

1.  Determine what task you want the model to perform.
2.  Filter models by task.
3.  Consider the required language.
4.  Check the license.
5.  Consider the model size/number of parameters.
6.  Look at downloads and likes.
7.  Check whether the model has been recently updated.
8.  Examine the model documentation before using it.

For example, if you only want models smaller than a certain size, you can filter by parameter count.

* * *

# 7\. Key Concepts to Remember

### Hugging Face has three core areas:

| Area | Purpose |
| --- | --- |
| **Models** | Find and use open-source AI models |
| **Datasets** | Find and share datasets |
| **Spaces** | Host and share AI applications |

### Important mental model

**GitHub → primarily source code**

**Hugging Face → models + datasets + AI applications**

* * *

# Key Takeaways

-   Week 3 begins the **open-source AI** portion of the course.
-   Hugging Face is a major hub for open-source AI.
-   The three most important parts of the platform are **Models, Datasets, and Spaces**.
-   Hugging Face contains a very large number of models, so filtering is essential.
-   Models can be evaluated using task, language, license, size, likes, downloads, and update history.
-   Hugging Face also provides a large collection of datasets.
-   **Spaces** allow AI applications to be hosted and shared.
-   Gradio applications can be deployed to Spaces using `gradio deploy`.
-   A **free Hugging Face account** is sufficient for the course.