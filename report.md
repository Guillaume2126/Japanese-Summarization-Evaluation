# Dataset Report

## Dataset Overview

The dataset consists of two aligned collections:

- **Articles dataset**: 50 Japanese news articles, each containing an `article_id`, URL, title, and full text.
- **Summaries dataset**: 250 summaries linked to the articles via `article_id`, with exactly 5 summaries per article.

## Data Analysis

- No missing values in either dataset.
- Perfect alignment between articles and summaries (5 summaries per article).

However, only **226 out of 250 summaries are unique**, indicating **24 duplicate summaries**. This suggests that some generation methods may produce repeated outputs, reducing diversity in the evaluation set.

Overall, the dataset is structurally clean, but the presence of duplicates highlights limitations in diversity that may affect evaluation robustness.

## Summary Statistics

- Minimum characters per summary: 22
- Maximum: 302
- Mean: 126

### Sentence Distribution

- 1 sentence: 23.2%
- 2 sentences: 31.6%
- 3 sentences: 45.2%
- 4+ sentences: 0%

## Observations

Some summaries are identical across different articles (e.g. `53174534` and `48116477`).  
This is not systematic but occurs in some cases.

## Failure Modes (Manual Inspection)

- Hallucinations: incorrect or fabricated facts
- Contradictions: inconsistent summaries for the same article
- Irrelevance: summaries unrelated to the source article
- Truncation: incomplete or cut-off sentences
- Low specificity / duplication: identical summaries across articles
- Faithfulness errors: subtle factual distortions (numbers, entities, locations)
- Salience errors: missing key information
- Poor compression: overly long or insufficiently condensed summaries
- Noise inclusion: metadata, captions, or non-content text

## Quality Dimensions

### Faithfulness
Factual consistency with the source article (absence of hallucinations)

### Relevance
Semantic similarity between article and summary

### Salience
Coverage of important named entities and key information

### Conciseness
Degree of compression relative to the source

### Truncation
Detection of incomplete or cut-off sentences

### Noise
Presence of non-informative content

# Scoring Framework

## Quality Dimensions

- Faithfulness
- Relevance
- Salience
- Conciseness
- Truncation
- Noise

## Methodology

### Semantic Similarity
- Model: `paraphrase-multilingual-mpnet-base-v2`
- Used for relevance and redundancy estimation

### Japanese Processing
- Tokenization: SudachiPy
- Entity extraction: heuristic (kanji-based patterns)

## Score Construction

### Feature Extraction

Each summary is evaluated using:

- Article–summary semantic similarity
- Hallucination proxy score
- Entity coverage
- Compression ratio
- Noise detection
- Truncation detection

### Normalization

- Faithfulness = 1 - hallucination_score
- Salience = entity recall
- Conciseness = normalized compression ratio
- Binary flags converted into penalties

### Global Score

Final weighted score:

- Faithfulness: 30%
- Salience: 20%
- Relevance: 15%
- Conciseness: 10%
- Truncation: 10%
- Noise: 5%

## Outputs

Each summary receives:

- Global score (0–1)
- Per-dimension scores
- Label:
  - excellent
  - good
  - medium
  - poor

## Validation

Weights were tuned iteratively to balance the importance of each dimension.

### Results

- Score distributions show sufficient variance
- Global score correlates positively with relevance and salience
- Global score correlates negatively with hallucination

### Label distribution (250 summaries)

- medium: 137
- good: 91
- poor: 22

### Sanity Checks

- High score + high hallucination: 0 cases
- Low score + high relevance: 0 cases

## Additional Insights

- Score distribution is slightly concentrated
- Compression ratios vary widely across summaries
- Most summaries are semantically aligned with source articles
- Some summaries show low relevance (topic drift or hallucination)

## Limitations

## Limitations

- **Heuristic-based metrics**: Several components rely on heuristics (e.g. entity extraction using kanji patterns, hallucination proxy), which may introduce noise and reduce robustness.

- **Weak supervision for quality dimensions**: Some dimensions such as faithfulness, salience, and noise are indirectly estimated rather than explicitly annotated, which may lead to misclassification.

- **Score calibration sensitivity**: The final weighted score depends on manually tuned weights, which may not generalize well to other datasets or tasks.

- **Notebook-centric development**: The current implementation relies heavily on Jupyter notebooks, which can hinder reproducibility, deployment, and maintainability. This setup also encourages code duplication across notebooks instead of reusable modular scripts. Transitioning to structured Python scripts or packages would improve scalability, reduce redundancy, and facilitate integration into production environments.

## Additional Notes

- Some manual checks used French translation for a better understanding.
- 
- AI was used to refactor notebook code (especially validation notebook) and correct some sentences.
- These two librairies were used using AI (how to use it):
  - translatepy
  - sudachipy

AI assistance also helped identify:
- Poor compression
- Noise inclusion