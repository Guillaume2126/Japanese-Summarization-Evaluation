The dataset consists of two aligned collections:

Articles dataset: 50 Japanese news articles, each containing an article_id, URL, title, and full text.
Summaries dataset: 250 summaries linked to the articles via article_id, with exactly 5 summaries per article.

The dataset is structurally consistent, with:

no missing values in either dataset
perfect alignment between articles and summaries (5 summaries per article)

However, we observe that only 226 out of 250 summaries are unique, indicating the presence of 24 duplicate summaries. This suggests that some generation methods may produce repeated outputs, potentially reducing diversity in the evaluation set.

Overall, while the dataset is clean in terms of structure and completeness, the presence of duplicate summaries highlights potential limitations in diversity that may impact the robustness of evaluation across systems.


The minimum of character for a summary is 22 and the maximum is 302. With a mean of 126. 

Regarding the sentences, 23.2% of summaries have 1 sentence, 31.6% 2 and almost half (45.2%) have 3 sentences. No summary more than 4 sentences or more.

Some summary are the same but for different articles ! (ex: 53174534 and 48116477). It is not everytime the case and sometimes, it can be the same id.


Through manual inspection, we identified several recurring failure modes:

Hallucinations – summaries introduce incorrect or fabricated facts.
Contradictions – summaries of the same article disagree on key facts.
Irrelevance – some summaries describe entirely different articles.
Truncation – incomplete or cut-off sentences.
Low specificity / duplication – identical summaries across articles.
Faithfulness errors – subtle distortions (numbers, locations, actors).
Salience errors – missing the main point of the article.
Poor compression – overly long or insufficiently condensed summaries.
Noise inclusion – presence of captions, reporter notes, or metadata.

What quality dimensions matter ?
Faithfulness → LLM-as-a-judge or NLI (entailment between article and summary)
Coverage → Named Entity Recall (NER overlap between article and summary)
Conciseness → length penalty (number of tokens or sentences, especially >3 sentences)


The five summaries per article differ along several dimensions, including factual faithfulness, level of information coverage, conciseness, and specificity. Some summaries are fully faithful and well-balanced, while others are truncated, overly generic, or contain hallucinated details.
