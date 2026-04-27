# Japanese-Summarization-Evaluation

# LLM Application Evaluation — Japanese News Summarization

## Overview

This project implements an evaluation framework for a Japanese news article summarization system.  
Given a news article, the task is to assess the quality of multiple candidate summaries (≤ 3 sentences).

The goal is not to build a summarizer, but to design and validate an evaluation system capable of distinguishing high-quality summaries from low-quality ones across multiple dimensions.

---

## Repository Structure

submission/
├── README.md
├── report.md
├── scores.jsonl
├── requirements.txt
├── code/
│   ├── data_analysis.ipynb
│   ├── scoring.ipynb
│   ├── validation.ipynb
├── data/
│   ├── articles.jsonl
│   ├── summaries.jsonl

---

## How to Run

### 1. Install dependencies

pip install -r code/requirements.txt

Main dependencies:
- sentence-transformers
- sudachipy
- numpy
- pandas
- matplotlib
- seaborn
- translatepy

---

### 2. Generate scores

Run:

jupyter notebook code/scoring.ipynb

This will:
- Load articles and summaries
- Compute multiple quality dimensions per summary
- Aggregate them into a weighted global score
- Export results to scores.jsonl

Each entry includes:
- summary_id
- article_id
- relevance
- hallucination
- salience
- compression_ratio
- compression_score
- truncation
- noise
- global_score
- label (excellent / good / medium / poor)

---

### 3. Validation

Run:

jupyter notebook code/validation.ipynb

This notebook:
- Ranks summaries by score
- Displays best and worst examples
- Translates samples (Japanese → French)
- Performs statistical validation of metrics
- Checks correlations and consistency

---

### 4. Data exploration

Run:

jupyter notebook code/data_visualization.ipynb

Used for:
- Exploring dataset structure
- Identifying failure modes
- Informing metric design decisions

---

## Evaluation Design

The evaluation decomposes summary quality into multiple dimensions:

### Faithfulness (Hallucination)
Detects unsupported information using:
- Token overlap (Sudachi tokenizer)
- Entity mismatch (kanji-based extraction heuristic)

---

### Relevance
Semantic similarity between article and summary using:
sentence-transformers/paraphrase-multilingual-mpnet-base-v2

---

### Salience
Measures whether key entities from the article are preserved in the summary.

---

### Conciseness
Compression ratio (summary vs article length), normalized using percentiles.

---

### Structural Quality
- Truncation detection (missing sentence-ending punctuation)
- Noise detection (journalistic artifacts, metadata, etc.)

---

## Final Scoring

Weighted aggregation:

Faithfulness: 0.30  
Relevance: 0.15  
Salience: 0.20  
Conciseness: 0.10  
No truncation: 0.10  
No noise: 0.05  

Final score:
global_score ∈ [0,1]

Mapped to labels:
- excellent
- good
- medium
- poor

---

## Validation Strategy

### 1. Correlation analysis
- hallucination should negatively correlate with score
- relevance and salience should positively correlate

---

### 2. Distribution analysis
- score histograms
- label separation
- outliers

---

### 3. Qualitative inspection
- Top 5 summaries
- Bottom 5 summaries
- Human-readable translations

---

### 4. Consistency checks
- High score + high hallucination cases
- Low score + high relevance cases

---

## Key Insights

- Semantic similarity alone is insufficient
- Hallucination detection is essential
- Entity overlap is a strong signal in Japanese summarization
- Compression normalization is required for fairness
- Multi-dimensional scoring is more robust than single metric

---

## Limitations

- Entity extraction is heuristic (regex-based)
- Semantic model may miss fine-grained factual errors
- No explicit fact-checking against external sources
- Reference summaries are noisy (XL-Sum dataset limitations)

---

## AI Usage

AI tools were used to:
- Assist in metric design exploration
- Help refine weighting strategy
- Support debugging and structuring of notebooks

Final design decisions were made manually through iterative experimentation.

---

## Notes

- Data is in Japanese, but analysis can be in English or French.
- Evaluation framework is modular and extensible for future improvements (e.g. LLM-based fact verification).