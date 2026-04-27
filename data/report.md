📊 Dataset Report
📦 Dataset Overview

The dataset consists of two aligned collections:

Articles dataset: 50 Japanese news articles, each containing an article_id, URL, title, and full text.
Summaries dataset: 250 summaries linked to the articles via article_id, with exactly 5 summaries per article.
🔍 Data Analysis
No missing values in either dataset.
Perfect alignment between articles and summaries (5 summaries per article).

However, we observe that only 226 out of 250 summaries are unique, indicating the presence of 24 duplicate summaries. This suggests that some generation methods may produce repeated outputs, potentially reducing diversity in the evaluation set.

Overall, while the dataset is clean in terms of structure and completeness, the presence of duplicate summaries highlights potential limitations in diversity that may impact the robustness of evaluation across systems.

📏 Summary Statistics
Minimum number of characters per summary: 22
Maximum: 302
Mean: 126
🧾 Sentence Distribution
23.2% of summaries contain 1 sentence
31.6% contain 2 sentences
45.2% contain 3 sentences
No summary contains more than 4 sentences
⚠️ Observations

Some summaries are identical but correspond to different articles (e.g., 53174534 and 48116477).
This is not always the case, and sometimes duplicates occur within the same article.

🚨 Failure Modes (Manual Inspection)

Through manual inspection, several recurring failure modes were identified:

Hallucinations – summaries introduce incorrect or fabricated facts
Contradictions – summaries of the same article disagree on key facts
Irrelevance – some summaries describe entirely different articles
Truncation – incomplete or cut-off sentences
Low specificity / duplication – identical summaries across articles
Faithfulness errors – subtle distortions (numbers, locations, actors)
Salience errors – missing the main point of the article
Poor compression – overly long or insufficiently condensed summaries
Noise inclusion – presence of captions, reporter notes, or metadata
🎯 Quality Dimensions

The following dimensions are used to evaluate summary quality:

1. Faithfulness

Measures factual consistency with the source article (absence of hallucinations)

2. Relevance

Semantic similarity between article and summary

3. Salience

Coverage of important information via named entities

4. Conciseness

Degree of compression relative to the source article

5. Truncation

Detection of incomplete or cut-off sentences

6. Noise

Presence of non-informative content (metadata, labels, captions)

🧾 Scoring Framework
🧠 1. Quality Dimensions

The evaluation framework is based on the 6 dimensions listed above.

⚙️ 2. Methodology
🔹 Semantic Similarity
Model: paraphrase-multilingual-mpnet-base-v2
Used for relevance and redundancy estimation
🔹 Japanese Processing
Tokenization using SudachiPy
Heuristic extraction of entities (kanji-based patterns)
🧮 3. Score Construction
Step 1 — Feature Extraction

Each summary is evaluated using:

Article–summary semantic similarity
Hallucination proxy score
Entity coverage
Compression ratio
Noise and truncation detection
Step 2 — Normalization

Raw metrics are transformed into interpretable scores:

faithfulness = 1 - hallucination_score
salience = entity recall
conciseness = distribution-based normalization of compression ratio
Binary flags converted into penalties
Step 3 — Global Scoring

Final weighted score:

Faithfulness: 30%
Salience: 20%
Relevance: 15%
Conciseness: 10%
Truncation: 10%
Noise: 5%
🏁 4. Outputs

Each summary is assigned:

A global score (0–1)
Per-dimension scores
A qualitative label:
excellent
good
medium
poor
✅ Validation

Weights were adjusted iteratively to find a balanced evaluation, as not all dimensions are equally relevant.

Score distributions show sufficient variance, indicating that the evaluator can meaningfully distinguish between summaries rather than collapsing to a narrow range.
The global score correlates positively with relevance and salience, and negatively with hallucination, matching expected qualitative behavior.
📊 Label Distribution (250 summaries)
medium: 137
good: 91
poor: 22
🔎 Sanity Checks
High score + high hallucination: 0 cases
Low score + high relevance: 0 cases
📉 Additional Insights
The score distribution remains somewhat concentrated, suggesting limited discriminative power.
Compression ratios reveal variability in summarization strategies, with some summaries being highly compressed while others are more verbose.
While most summaries remain semantically aligned with the source, a subset shows low relevance, indicating failure modes such as topic drift or hallucination.
⚠️ Limitations

(To be completed — suggested: evaluation bias, heuristic limitations, language-specific issues, etc.)

📝 Additional Notes
At certain steps, French translations were used to better assess summary relevance.
AI was used to clean and refactor parts of the notebooks, especially in the "Validation" notebook.
The following libraries were used:
translatepy
sudachipy

AI assistance also helped identify additional failure modes such as poor compression and noise inclusion.