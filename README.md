# OC Restaurant Vibe Classifier
This project focuses on fine-tuning a small local NLP model to classify the “vibe” or atmosphere of Orange County restaurants using restaurant descriptions and review snippets.


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

---

## Problem Statement

Restaurant recommendations are often difficult because users care about more than cuisine alone. Social atmosphere, environment, and intended use heavily influence dining decisions.

For example:
- A coffee shop may be ideal for studying but not for group dinners.
- A restaurant may be excellent for date nights but not family-friendly.
- Some locations are trendy social spots while others are quiet and casual.

The objective of this project is to train a local NLP model capable of understanding restaurant descriptions and predicting these vibe categories automatically.

---

## Example Inputs and Outputs

### Example 1

#### Input
"Dim lighting, craft cocktails, small plates, and a quiet outdoor patio."

#### Output
```json
{
  "primary_vibe": "date_night",
  "secondary_tags": ["upscale", "romantic"]
}
```


### Example 2

#### Input
"Bright cafe with coffee, Wi-Fi, pastries, and students working on laptops."

#### Output
```json
{
  "primary_vibe": "study_spot",
  "secondary_tags": ["casual", "quiet"]
}
```


### Example 3

#### Input
"Open late with loud music, large booths, and shareable appetizers."

#### Output
```json
{
  "primary_vibe": "group_friendly",
  "secondary_tags": ["late_night", "trendy"]
}
```


## Main NLP Task 
This project is frames as a supervised text classification problem. 

The model receives:
* restaurant descriptions
* review snippets
* restaurant atmosphere summaries
Potential sources include:

restaurant descriptions
public review snippets
manually written synthetic examples
The model predicts:
* a primary vibe category
* optionsl secondary vibe tags for a deeper description


## Dataset
The dataset will consist of Orange County restaurant descriptions and review snippets collected from publicly available restaurant information and manually created examples.

Each dataset example will contain:
* restaurant text description
* labeled vibe category

Example dataset row:
| text                                                   | label      |
| ------------------------------------------------------ | ---------- |
| "Modern interior with cocktails and intimate seating." | date_night |

Potential sources include:
* restaurant descriptions
* public review snippets
* manually written synthetic examples


## Model
The project will use a lightweight transformer model capable of running locally on consumer hardware.

Potential models include:

* DistilBERT
* MiniLM
* TinyBERT

The model will be fine-tuned locally using Hugging Face Transformers and PyTorch.


# Fine Tuning Approach 
The project will perform supervised fine-tuning for text classification.

The process includes:
1. preprocessing restaurant text
2. tokenization
3. train/validation/test split
4. local model fine-tuning
5. evaluation and error analysis


## Baselines 
The fine-tuned model will be compared against:
* the base pretrained model without fine-tuning
* a simple keyword/rules-based classifier

This comparison will help determine whether fine-tuning improves performance.


## Evaluation Metrics
The project will evaluate performance using:
* accuracy 
* precision
* recall
* F1-score
* confusion matrix analysis

Qualitative analysis will also be included to examine:
* successful predictions
* ambiguous examples 
* common failure cases 


## Project Goals 
The final system should:
1. run locally on a laptop 
2. classify restaurant vibes from natural language text 
3. outperform simple baseline method s
4. demonstrate effective domain-specific NLP fine-tuning 


## Repository Structure 

```
data/
notebooks/
src/
results/
report/
demo/
```


## Tech Stack 
* Python 
* PyTorch
* Hugging Face Transformers
* scikit-learn
* pandas
* matplotlib


## Author 
Natalie Huante | Chapman University | CPSC 543 Final Project | Spring 2026 