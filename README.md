# OC Restaurant Vibe Classifier

This project aims to perform local fine-tuning of DistilBERT for multi-class restaurant vibe classification using Yelp restaurant reviews and metadata.

The goal is to determine whether a restaurant is best described as:
- date night
- study spot
- casual hangout
- family-friendly
- upscale
- trendy
- late night
- group-friendly

The project is designed as a focused domain-specific NLP classification task rather than a general chatbot or recommendation engine.


## Author 
Natalie Huante | Chapman University | CPSC 543 Final Project | Spring 2026 


---


## Project Overview

This project explores whether transformer-based NLP models can classify the atmosphere or "vibe" of restaurants using Yelp review text and restaurant category metadata. 

The system was trained locally using DistilBERT and compared against a traditional NLP baseline using TF-IDF and Logistic Regression. 

The project focuses on restaurants located in :
* Santa Barbara
* Goleta
* Carpinteria
---

## NP Task

This project frames restaurant vibe prediction as a supervised multi-class text classification task.

### Input

Combined restaurant metadata and Yelp review text.

```
Categories: Coffee & Tea, Cafes
Review: Quiet cafe with students working on laptops and plenty of outlets.
```


### Output

One of the following vibe labels:

* `study_spot`
* `date_night`
* `casual_hangout`
* `family_friendly`
* ``upscale`
* `trendy`
* `late_night`


## Dataset

### Source 

The dataset was constructed using the Yelp Open Dataset, which contains large-scale Yelp business, review, tip, and user data in JSON format, including restaurant metadata and full review text. 

The dataset specifically pulled information from these two json files in the Open Dataset:
* `yelp_businesses.json`
* `yelp_reviews.json`

### Construction 

The pre-processing pipeline included the following steps:

1. Loading Yelp business and review datasets
2. Extracting restaurant-related businesses
3. Filtering restaurants to Santa Barbara region cities 
4. Removing irrelevant business and sprase entries
5. Merging restaurant and reviews data 
6. Construction NLP input text fields
7. Creating manually balanced vibe label samples

### Final Dataset 

The final dataset contains roughly 200 labeled examples that are balanced across seven restaurant vibe lavel categories and split into train / validation / test sets. 


## Models

### Primary Model

**DistilBERT** *(used for sequence classification)* is the main model used in this project. The model was fine-tuned, trained using Hugging Face Transformers, optimized using PyTorch, and evaluated using supervised classification metrics. 

### Baseline Model 

A traditional machine learning baseline was implemented using TF-IDF vectorization and Logistic Regression classification. This was used to compare transformer performance against a classical NLP method. 


## Experiments 

Three DistilBERT fine-tuning experiments were tested. 

| Model | Learning Rate | Epochs | Notes | 
|---|---:|---:|---:|
| 1 | 3e-5 | 8 | baseline fine-tuning version |
| 2 | 2e-5 | 8 | added warmup and slower learning rate | 
| 3 | 3e-5 | 10 | extended training period | 


## Final Results 

### Evaluation Metrics

The models in this project were evaluated using several standard multi-class classification metrics:

| Metric | Purpose | 
| --- | --- | 
| Accuracy | the percentage of correctly classified examples | 
| Precision | how many predicted labels were correct | 
| Recall | how many true labels were successfully identified | 
| F1 Score | balances precision and recall into a single score | 

Because restaurant vibe labels can overlap semantically, the weighted F1-score was considered one of the most important metrics for comparing model performance. 

Additional metrics included 
* confusion matrix analysis 
* qualitative prediction examples
* baseline comparison against TF-IDF + Logistic Regression


### DistilBERT Chosen Model 

| Metric | Score | 
| --- | --- |
| Accuracy | 0.67| 
| Weighted F1 | 0.68| 

### Baseline Model 

| Metric | Score | 
| --- | --- |
| Accuracy | 0.67| 
| Weighted F1 | 0.66|

The fine-tuned DistilBERT model slightly outpeformed the TF-IDF baseline while demonstrating stronger contextual understanding on semantically overlapping vibe categories. 


## Repository Structure 

```
oc-restaurant-vibe-classifier/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── splits/
│
├── demo/ 
|
├── notebooks/
│   ├── 01_dataset_creation.ipynb
│   └── 02_preprocessing_and_training.ipynb
│
├── results/
│   ├── confusion_matrix.png
│   ├── classification_report.txt
│   └── final_model/
│
├── report/
│   └── report.tex
|   └── report.pdf
|
├── results/
|   └── final_model 
|   └── model_output
|   └── model_output_exp2
|   └── model_output_exp3
|   └── classification_report.txt
|   └── confusion_matrix.png
|   └── demopredictions.csv
│
├── requirements.txt
├── .gitignore
└── README.md
```


## Local Environment Setup

This project is designed to run locally on a laptop, as required by the final project guidelines.

### 1. Clone the repository

```bash
git clone https://github.com/nhuante/oc-restaurant-vibe-classifier.git
cd oc-restaurant-vibe-classifier
```

### 2. Setup Virtual Environment

Create virtual environment
```bash 
python -m venv venv
```


Activate it

```bash
# (Windows Powershell) 
.\venv\Scripts\Activate.ps1

# (Mac/Linux) 
source venv/bin/activate
```


### 3. Install Dependencies 

```bash 
pip install -r requirements.txt
```


### 4. Verify Setup 

```bash 
python -c "import torch, transformers, sklearn, pandas; print('Environment works')"
```

### 5. Launch Jupyter Notebook 

```bash 
jupyter notebook
```


## Tech Stack 
* Python 
* PyTorch
* Hugging Face Transformers
* scikit-learn
* pandas
* Jupyter Notebook
* matplotlib


## Key Findings 
* Traditional NLP baselines can perform competitively on small keyword-heavy datasets.
* DistilBERT demonstrated stronger contextual understanding on overlapping vibe categories.
* Restaurant atmosphere classification contains significant semantic subjectivity.
* Small datasets can limit the advantages of transformer-based models.


## Future Improvements 

Potential future improvements include:
* expanding the labeled dataset
* adding more geographically diverse restaurants
* incorporating menu descriptions and restaurant metadata
* experimenting with larger transformer models
* testing parameter-efficient fine-tuning methods such as LoRA
* building a restaurant recommendation interface on top of the classifier