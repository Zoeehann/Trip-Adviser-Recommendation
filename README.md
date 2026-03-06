# TripAdvisor Recommendation System

A text-driven place recommendation system based on TripAdvisor review similarity

## 📋 Project Overview

This project builds a text-driven recommendation system that analyzes TripAdvisor review text to recommend similar tourist destinations.

**Core Hypothesis**: Similar tourist experiences are described using similar vocabulary

**Key Achievements**:
- Implemented BM25 baseline and TF-IDF+Cosine improved models
- Achieved 85%+ recommendation accuracy (Success@1)
- Improved model outperforms baseline on all metrics

---

## 📊 Dataset

- **Reviews**: 340,385 reviews
- **Places**: 2,161 unique places
- **Languages**: Multilingual (primarily English and French)
- **Place Types**: Hotels (H), Restaurants (R), Attractions (A), Attraction Products (AP)

**Data Files**:
```
reviews83325.csv          # Review data
Tripadvisor.csv          # Place metadata
AttractionSubCategorie.csv
AttractionSubType.csv
cuisine.csv
dietary_restrictions.csv
restaurantType.csv
```

---

## 🚀 Quick Start

### 1. Environment Setup

```bash
# Install dependencies
pip install -r requirements.txt

# Download NLTK data
python -c "import nltk; nltk.download('stopwords'); nltk.download('wordnet'); nltk.download('punkt')"
```

### 2. Execution Pipeline

```bash
# Step 0: Data diagnosis (optional)
python 0_diagnose_data.py

# Step 1: Data exploration (optional)
python 1_data_exploration.py

# Step 2: Data preprocessing
python 2_preprocessing.py
# Output: documents_by_place.csv

# Step 3: TF-IDF balancing
python 3_balance_tfidf.py
# Output: documents_by_place_tfidf.csv

# Step 4: Train BM25 model
python 4_baseline_bm25.py
# Output: bm25_model.pkl

# Step 5: Train TF-IDF+Cosine model
python 5_tfidf_cosine_model.py
# Output: tfidf_cosine_model.pkl

# Step 6: Evaluate BM25 (optional)
python 6_evaluation_standalone.py
# Output: evaluation_results_bm25.json

# Step 7: Compare both models
python 7_compare_models.py
# Output: model_comparison.json
```

---

## 📁 Project Structure

```
.
├── 0_diagnose_data.py              # Data diagnosis tool
├── 1_data_exploration.py           # Data exploration and visualization
├── 2_preprocessing.py              # Text preprocessing
├── 3_balance_tfidf.py              # TF-IDF balancing (key step)
├── 4_baseline_bm25.py              # BM25 baseline model
├── 5_tfidf_cosine_model.py         # TF-IDF+Cosine improved model
├── 6_evaluation_standalone.py      # Standalone BM25 evaluation
├── 7_compare_models.py             # Model comparison evaluation
├── requirements.txt                # Python dependencies
├── README.md                       # This file
└── reports/
    ├── TripAdvisor_Report_Chinese.docx
    └── TripAdvisor_Report_English.docx
```

---

## 🔧 Data Processing Pipeline

### 1. Text Preprocessing
- Lowercase conversion
- Remove HTML, URLs, punctuation, numbers
- Tokenization
- Stopword removal (English + French)
- Lemmatization

### 2. Document Aggregation
- Concatenate all reviews per place into a single document

### 3. TF-IDF Balancing ⭐ (Key Step)
- Use TF-IDF to select top 100 keywords per place
- Solve review quantity imbalance problem (1 ~ 71,257 reviews/place)
- Ensure fair comparison

### 4. Data Split
- Training set: 50% (1,080 places)
- Test set: 50% (1,081 places)

---

## 🤖 Models

### BM25 (Baseline)
- **Principle**: Probabilistic ranking function
- **Features**: Term frequency + IDF + length normalization
- **Parameters**: k1=1.5, b=0.75

### TF-IDF + Cosine Similarity (Improved Model)
- **Principle**: Vector space model + cosine similarity
- **Features**: 
  - Uses bigrams (1,2) to capture phrases
  - max_features = 5000
  - Better semantic matching
- **Advantages**: Outperforms BM25 on both evaluation levels

---

## 📈 Evaluation Results

### Level 1 (Type Matching: H/R/A/AP)

| Model | Mean Ranking Error | Success@1 | Success@10 |
|-------|-------------------|-----------|------------|
| BM25 | 0.435 | 85.09% | 98.98% |
| TF-IDF+Cosine | **0.400** | **85.66%** | **99.26%** |

### Level 2 (Category Matching)

| Model | Mean Ranking Error | Success@1 | Success@10 |
|-------|-------------------|-----------|------------|
| BM25 | 0.726 | 81.08% | 97.95% |
| TF-IDF+Cosine | **0.694** | **82.67%** | **98.23%** |

**Conclusion**: TF-IDF+Cosine outperforms BM25 on all metrics

---

## 📦 Dependencies

```txt
pandas>=1.5.0
numpy>=1.23.0
scikit-learn>=1.2.0
rank-bm25>=0.2.2
nltk>=3.8
tqdm>=4.65.0
matplotlib>=3.6.0
seaborn>=0.12.0
```

---

## 💡 Key Technical Points

1. **TF-IDF Balancing Strategy** (Following instructor's recommendation)
   - Select top 100 keywords per place
   - Solve data imbalance problem
   - Improve recommendation quality

2. **Bigram Features**
   - Capture multi-word phrases (art museum, Italian restaurant)
   - Improve fine-grained category matching accuracy

3. **Two-Level Evaluation System**
   - Level 1: Coarse-grained type matching
   - Level 2: Fine-grained attribute matching

---

## 🎯 Main Achievements

1. ✅ Implemented and compared two recommendation models
2. ✅ Achieved 85%+ recommendation accuracy
3. ✅ Validated the effectiveness of text similarity recommendation
4. ✅ TF-IDF+Cosine model outperforms BM25 baseline

---

## 📝 Reports

Complete project reports are in the `reports/` directory:
- `TripAdvisor_Report_Chinese.docx`
- `TripAdvisor_Report_English.docx`

Reports include:
- Detailed data processing pipeline
- Model design and parameter specifications
- Complete evaluation results and analysis
- Model comparison and discussion

---

## ⚠️ Important Notes

1. **Data Files**: Ensure all CSV files are in the same directory
2. **Memory**: Preprocessing may require 2-4 GB RAM
3. **Time**: Complete pipeline takes approximately 15-20 minutes
4. **NLTK**: First run requires downloading language resources

---

## 🔍 FAQ

**Q: How to test with partial data?**
```python
# Modify in 2_preprocessing.py
reviews_df = pd.read_csv('reviews83325.csv', nrows=10000)
```

**Q: How to adjust number of TF-IDF selected words?**
```python
# Modify in 3_balance_tfidf.py
TOP_N_WORDS = 100  # Change to 50, 150, 200, etc.
```

**Q: How to use only English reviews?**
```python
# Modify in 2_preprocessing.py
USE_ENGLISH_ONLY = True
```

---

## 📧 Contact

- Project Author: [Your Name]
- Course: Information Retrieval
- Date: March 2026

---

## 📄 License

This project is for academic research and coursework only
