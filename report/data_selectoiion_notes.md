
# Dataset Comparison and Selection

# 1. Yelp Open Dataset

## Overview
The Yelp Open Dataset contains large-scale Yelp business, review, tip, and user data in JSON format, including restaurant metadata and full review text.

## Pros
- Very large and academically recognized dataset
- Contains rich review text and business metadata
- Includes useful restaurant-related attributes:
  - categories
  - ratings
  - hours
  - reviews
  - tips
- Strong authenticity and diversity of language

## Cons
- Extremely large and computationally heavy
- Requires extensive preprocessing and JSON parsing
- Not focused specifically on Orange County
- Significant filtering and cleaning required
- More suited for longer research projects with more time

## Compatibility with Project
High academic quality, but ultimately too broad and engineering-heavy for the project timeline and scope.

---

# 2. Orange, CA Kaggle Restaurant Dataset (Selected Dataset)

## Overview
This dataset contains restaurant and business metadata specifically focused on Orange, California, including descriptive business attributes and atmosphere-related information.

## Pros
- Already geographically narrowed to the project domain
- Much easier preprocessing pipeline
- Contains highly relevant atmosphere-related fields:
  - description
  - categories
  - atmosphere
  - crowd
  - dining options
  - popular for
- Better aligned with the restaurant vibe classification task
- Smaller and more manageable for local experimentation
- Easier manual labeling and dataset balancing

## Cons
- Smaller dataset size
- Some fields may contain missing or inconsistent values
- May require synthetic augmentation to balance labels
- Less raw review diversity than the full Yelp dataset

## Compatibility with Project
Most compatible with the project goals and timeline. It provides relevant restaurant atmosphere metadata while keeping preprocessing manageable for a local NLP fine-tuning workflow.

---

# 3. Yelp Restaurant Reviews Dataset

## Overview
This dataset primarily focused on restaurant review text and ratings without sufficient atmosphere or contextual restaurant metadata.

## Pros
- Easy to use
- Already structured in tabular format
- Contains natural language restaurant reviews
- Useful for general sentiment or review analysis tasks

## Cons
- Limited atmosphere/contextual metadata
- Less useful for extracting restaurant “vibes”
- Reviews alone often lacked enough information to confidently assign vibe labels
- Not geographically focused on Orange County
- Less comprehensive than the selected Kaggle dataset

## Compatibility with Project
Moderately compatible, but ultimately not selected because it lacked the detailed atmosphere-oriented features needed for reliable vibe classification.

---

# Why a Fully Manual Dataset Was Not Selected

A fully manually created dataset was considered during the project planning phase. However, this approach was ultimately not selected for several reasons.

## Limitations of a Fully Manual Dataset
- Creating hundreds of realistic restaurant descriptions manually would have been extremely time-consuming.
- A manually authored dataset could unintentionally introduce repetitive writing styles or labeling bias.
- Synthetic-only examples may not accurately reflect the diversity and variability of real-world restaurant language.
- The project aimed to train and evaluate the model on more authentic restaurant-related text data.

## Advantages of Using Real Restaurant Data
Using a real-world dataset provided:
- more natural language diversity
- more realistic restaurant descriptions
- authentic atmosphere and dining terminology
- improved credibility for evaluation and analysis

The selected Orange, CA Kaggle dataset provided a strong balance between authenticity, relevance, and manageable preprocessing complexity while still allowing limited synthetic augmentation where necessary for label balancing.

---

# Final Dataset Selection

The Orange, CA Kaggle dataset was selected because it provided the best balance between:
- domain relevance
- manageable preprocessing complexity
- atmosphere-related metadata
- compatibility with local model fine-tuning
- feasibility within the project timeline

The dataset also aligned closely with the project’s NLP task of classifying restaurant atmosphere and social “vibe” categories from textual restaurant descriptions.
