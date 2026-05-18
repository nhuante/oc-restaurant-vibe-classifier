# OC Restaurant Vibe Classifier — Detailed Project Plan

## Phase 1 — Project Setup (1–2 hours)

### Goals
- Create GitHub repo
- Setup Python environment
- Install dependencies
- Create folder structure
- Write README

### Deliverables
- working repo
- requirements.txt
- basic project structure

---

# Phase 2 — Define Labels and Dataset (4–6 hours)

## Goals
Create a clean, focused classification dataset.

### Final vibe labels
Likely:
- date_night
- study_spot
- casual_hangout
- family_friendly
- upscale
- trendy
- late_night
- group_friendlyv

### Dataset target
Aim for:
- 100–200 labeled examples

### Dataset format
CSV:
```csv
text,label
```

### Data collection sources
- OC restaurant descriptions
- public review snippets
- manually written examples
- synthetic augmentation

### Important goal
Keep labels balanced.

---

# Phase 3 — Baseline Model (2–3 hours)

## Goals
Build a simple baseline for comparison.

### Option A — Rules-based classifier
Keyword matching:
- “cocktails” → date_night
- “wifi” → study_spot
- “families” → family_friendly

### Option B — Base transformer without fine-tuning
Evaluate pretrained model performance directly.

### Deliverables
- baseline accuracy/F1
- baseline predictions

---

# Phase 4 — Fine-Tuning (4–6 hours)

## Goals
Train a lightweight transformer locally.

### Recommended models
- DistilBERT
- MiniLM

### Tasks
- tokenize dataset
- train classifier
- validate performance
- save trained model

### Deliverables
- trained local model
- training logs
- evaluation outputs

---

# Phase 5 — Evaluation and Error Analysis (2–3 hours)

## Goals
Analyze performance quantitatively and qualitatively.

### Quantitative evaluation
- accuracy
- precision
- recall
- F1
- confusion matrix

### Qualitative evaluation
Analyze:
- correct predictions
- ambiguous predictions
- failure cases
- overlapping vibe categories

### Deliverables
- metrics tables
- confusion matrix
- prediction examples

---

# Phase 6 — Paper Writing (4–5 hours)

## Required sections
- Abstract
- Introduction
- Problem Definition
- Dataset
- Model and Fine-Tuning Method
- Experimental Setup
- Results
- Error Analysis
- Ethical Considerations
- Conclusion

### Include
- dataset examples
- metrics tables
- architecture discussion
- limitations

---

# Phase 7 — Demo Recording (1–2 hours)

## Demo should show
- project motivation
- dataset examples
- baseline vs fine-tuned model
- live predictions
- evaluation results
- strengths and weaknesses

### Final deliverables
- PDF paper
- recorded demo
- GitHub repository
- trained local model/codebase
