# Star-Rating Prediction for Turkish E-Commerce Reviews with a Multi-Agent Workflow

**BLM3120 – Term Project Proposal (Fall 2026)**
**Group:** İsa Koçan – 22011056 · Bilal Efe Uysal – 22011031

**Why?** Customer reviews are valuable web content, but their star ratings are noisy, and Turkish user text is messy: typos, missing Turkish characters, emojis. Predicting the rating from the text alone is a practical web content mining problem.

**What?** We will predict the 1–5 star rating of a Turkish product review (5-class classification) with **supervised learning**, and compare **Decision Tree** (baseline), **k-NN** (alternative: lazy, instance-based) and **Random Forest** (advanced: an ensemble of trees).

**How?** We will build the application with **smolagents**, using **NVIDIA Nemotron 3.5 Lightning** (NVIDIA Build API) as the LLM. A main agent asks the user to approve its plan, then runs six task agents one by one:
1. Data Transformation
2. Data Filtering: balanced sample of 10k reviews per star, 80/20 train/test split
3. Feature Extraction: TF-IDF and simple text features
4. Outlier Detection
5. Algorithm Execution
6. Evaluation

All computations are done in Python tools; the LLM only sees counts and summaries. We will measure accuracy, macro-F1 and confusion matrices, select parameters with 5-fold cross-validation, and test significance with a t-test. A **Streamlit** GUI will provide the three required screens.

**Dataset.** *MüşteriYorumları*: 103,920 Turkish reviews from Hepsiburada and Trendyol, with review text and a 1–5 star label, in JSONL format.
https://huggingface.co/datasets/turkish-nlp-suite/MusteriYorumlari
The reviews are real user-generated web content and come with labels, so the dataset fits supervised learning directly.

**When?**
| Dates | Milestone |
|---|---|
| 23 – 29 Nov | Design decisions, data exploration, proposal (due 30 Nov) |
| 30 Nov – 3 Dec | Data tools and pipeline |
| 3 – 6 Dec | Agents |
| 6 – 9 Dec | GUI |
| 9 – 11 Dec | Experiments |
| 11 – 13 Dec | Report and submission |
