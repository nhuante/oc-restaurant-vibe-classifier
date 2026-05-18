# OC / Santa Barbara Restaurant Vibe Classifier — Project Recap

## Project Goal
Build an NLP-based restaurant vibe classifier using Yelp review text and restaurant metadata.

The model predicts one of the following restaurant vibe labels:

```python
VIBE_LABELS = [
    "date_night",
    "study_spot",
    "casual_hangout",
    "family_friendly",
    "upscale",
    "trendy",
    "late_night",
]
```

The project uses:
- Yelp Open Dataset
- DistilBERT transformer model
- Supervised text classification
- Fine-tuning using Hugging Face Transformers

---

# 1. Initial Dataset Planning

## Original Idea
The project initially focused on:
- Orange County restaurant recommendations
- Restaurant vibe detection
- Wait time / reservation prediction
- Cuisine and craving-based recommendations

## Dataset Decision
The original Orange County dataset from Kaggle required a paid license (~$1000), so the project pivoted to using the official Yelp Open Dataset.

Files used:
- `yelp_businesses.json`
- `yelp_reviews.json`

---

# 2. Environment Setup

## Repository Setup
Created:
- GitHub repository
- project folder structure
- README.md

## Python Environment
Created a local virtual environment:

```bash
python -m venv venv
```

Activated in PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

Installed libraries:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn torch transformers datasets accelerate jupyter
```

---

# 3. Dataset Exploration and Cleaning

## Loaded Yelp Data

```python
businesses = pd.read_json(business_path, lines=True)
reviews = pd.read_json(reviews_path, lines=True)
```

## Null Value Inspection
Businesses dataset:
- `hours` and `attributes` contained null values
- core restaurant metadata was complete

Reviews dataset:
- no null values

---

# 4. Category Analysis

Extracted all unique Yelp business categories.

Steps:
1. Split comma-separated category strings
2. Flatten categories
3. Count frequencies
4. Export category counts CSV

Used this to construct restaurant-focused filtering rules.

---

# 5. Restaurant Filtering Pipeline

## Included Food Categories
Examples:
- Restaurants
- Cafes
- Coffee & Tea
- Bars
- Nightlife
- Breakfast & Brunch
- Sushi Bars
- Cocktail Bars
- Pizza
- Mexican
- Burgers

## Excluded Categories
Examples:
- Grocery
- Convenience Stores
- Strip Clubs
- Tobacco Shops
- Hotels
- Music Venues
- Shopping

---

# 6. Geographic Filtering

Originally intended for Orange County.

Discovered Yelp dataset primarily contained:
- Santa Barbara
- Goleta
- Carpinteria

Final project scope:
- Santa Barbara region restaurant vibe classification

Filtered businesses:

```python
TARGET_CITIES = [
    "Santa Barbara",
    "Goleta",
    "Carpinteria"
]
```

---

# 7. Restaurant–Review Pair Construction

Merged restaurant metadata with Yelp reviews using:

```python
business_id
```

Result:
Each row represented:
- restaurant categories
- review text
- metadata

Example structure:

| categories | review | label |
|---|---|---|
| Coffee & Tea, Cafes | Quiet cafe with laptops and coffee | study_spot |

---

# 8. Supervised Label Design

Final vibe labels:

```python
VIBE_LABELS = [
    "date_night",
    "study_spot",
    "casual_hangout",
    "family_friendly",
    "upscale",
    "trendy",
    "late_night",
]
```

## Input Format

Constructed NLP inputs as:

```text
Categories: Coffee & Tea, Cafes | Review: Quiet cafe with students working on laptops.
```

---

# 9. Initial Labeling Attempt

Attempted automatic keyword-based labeling.

Problem:
- severe class imbalance
- most examples labeled as `late_night`

Example:

| Label | Count |
|---|---:|
| late_night | 107 |
| trendy | 0 |
| upscale | 2 |

Conclusion:
- automatic labeling alone was insufficient
- labels needed manual balancing

---

# 10. Balanced Dataset Construction

Used:
- category-driven candidate pools
- keyword searches
- manual curation strategy

Targeted balanced distribution:

| Label | Count |
|---|---:|
| date_night | 29 |
| study_spot | 29 |
| casual_hangout | 29 |
| family_friendly | 29 |
| upscale | 28 |
| trendy | 28 |
| late_night | 28 |

Final dataset size:

```text
200 labeled examples
```

---

# 11. Label Encoding

Transformer models require numerical labels.

Used:

```python
LABEL_TO_ID = {
    "date_night": 0,
    "study_spot": 1,
    "casual_hangout": 2,
    "family_friendly": 3,
    "upscale": 4,
    "trendy": 5,
    "late_night": 6
}
```

Created:
- `LABEL_TO_ID`
- `ID_TO_LABEL`

---

# 12. Train / Validation / Test Split

Used stratified splitting:

```python
train_test_split(..., stratify=df["label"])
```

Final split:

| Dataset | Percentage |
|---|---:|
| Train | 70% |
| Validation | 15% |
| Test | 15% |

---

# 13. Transformer Tokenization

Model used:

```text
DistilBERT (distilbert-base-uncased)
```

Tokenizer:

```python
DistilBertTokenizer
```

Tokenization settings:

```python
padding="max_length"
truncation=True
max_length=256
```

---

# 14. Hugging Face Dataset Conversion

Converted pandas dataframes into:

```python
Dataset.from_pandas(...)
```

Prepared tensors:
- input_ids
- attention_mask
- label

---

# 15. Experiment 1 — Baseline Fine-Tuning

## Configuration

```python
learning_rate = 3e-5
num_train_epochs = 8
batch_size = 8
weight_decay = 0.01
```

## Result

| Metric | Value |
|---|---:|
| Accuracy | 66.7% |
| Precision | 75.6% |
| Recall | 66.7% |
| F1 | 68.1% |

Observations:
- strong performance for small dataset
- some overlap between subjective vibe labels
- evidence of mild overfitting

---

# 16. Experiment 2 — Lower Learning Rate + Warmup

## Configuration

```python
learning_rate = 2e-5
warmup_ratio = 0.1
num_train_epochs = 8
```

## Important Discovery
Initially, Experiment 2 accidentally reused the already fine-tuned model from Experiment 1.

This was corrected by:
- creating a fresh DistilBERT instance
- retraining from scratch

## Final Clean Experiment 2 Result

| Metric | Value |
|---|---:|
| Accuracy | 46.7% |
| Precision | 47.4% |
| Recall | 46.7% |
| F1 | 44.8% |

Conclusion:
- lower learning rate converged too slowly
- Experiment 1 generalized better

---

# 17. Evaluation and Error Analysis

Generated:
- predictions
- classification report
- confusion matrix

## Strongest Classes

Perfect or near-perfect performance:
- study_spot
- casual_hangout

Reason:
- highly distinct vocabulary patterns

## Most Difficult Classes

Lower F1 scores:
- date_night
- trendy
- upscale

Reason:
- semantic overlap
- subjective interpretation
- similar nightlife language

## Important Observation
Errors were generally semantically meaningful.

Example confusions:
- upscale ↔ date_night
- trendy ↔ late_night
- trendy ↔ date_night

This suggests the model learned real social/atmospheric relationships rather than random patterns.

---

# 18. Current Project Status

Completed:
- dataset construction
- filtering and preprocessing
- supervised labeling
- balanced dataset engineering
- transformer fine-tuning
- multiple experiments
- evaluation metrics
- confusion matrix
- qualitative analysis

Remaining:
- final report writing
- presentation/demo preparation
- optional inference/demo examples
- final model export

---

# 19. Key Technical Stack

## Libraries
- pandas
- numpy
- scikit-learn
- PyTorch
- Hugging Face Transformers
- Hugging Face Datasets
- matplotlib

## Model
- DistilBERT (`distilbert-base-uncased`)

## Task Type
- supervised multi-class text classification

---

# 20. Main Takeaways

## NLP Concepts Demonstrated
- transformer fine-tuning
- supervised text classification
- tokenization
- train/validation/test splitting
- hyperparameter experimentation
- class balancing
- error analysis

## Major Challenges
- dataset imbalance
- subjective labels
- overlapping semantic classes
- limited dataset size

## Major Successes
- built custom NLP dataset
- successfully fine-tuned transformer locally
- achieved ~67% test accuracy on 7-class problem
- meaningful confusion matrix patterns
- successful experimental comparison

