# AIDS_Sem7_NLP_Experiment10: Exploratory Data Analysis (EDA) on a Text Corpus

**Name:** Girish Sakpal   
**UIN:** 231A011   
**Subject:** CSDOL7011 - Natural Language Processing  
**Performed On:** 08/10/2026

## Aim
Perform exploratory data analysis on a text corpus, visualizing important insights with tools like word clouds, frequency plots, and distribution plots.

## Problem Statement
Given a large collection of raw text (the 50,000-review IMDb movie review dataset), summarize its basic characteristics, clean it, and visualize its structure and vocabulary (review lengths, most frequent words with and without stop words, a word cloud, and common bigrams) to understand the data before any downstream task such as sentiment analysis or classification.

## Brief Theory
- **EDA** summarizes the main characteristics of a dataset using statistics and visualizations. For text, it turns unstructured data into insights that guide preprocessing and modeling.
- **Preprocessing** (tokenization, lowercasing, stop word removal, normalization) reduces noise so that meaningful words stand out in the plots.
- **Word clouds** show dominant terms (larger word = higher frequency), **frequency plots** give exact counts of top words, and **distribution plots** (e.g. a histogram of review lengths) show how a numeric text property is spread.
- **N-gram analysis** (e.g. bigrams) reveals common word pairs and phrases that single-word counts miss.
- Typical tools: Pandas, NLTK, scikit-learn, Matplotlib/Seaborn, and the WordCloud library.

## Implementation Explanation
1. Imported `pandas`, `datasets`, `CountVectorizer`, `matplotlib`, `Counter`, `WordCloud`, `re`, and loaded the IMDb dataset with `load_dataset("stanfordnlp/imdb")`.
2. Combined the **train** and **test** splits into one DataFrame, renamed `label` to `labels`, and checked shape and class balance.
3. Viewed the first rows with `df.head()`, then checked for **missing values** and **duplicate rows**.
4. Computed per-review statistics: character count, word count, unique word count, and average word length, and summarized them with `.describe()`.
5. Defined `clean_text()` to lowercase the text, strip HTML tags, keep only letters and whitespace, and collapse extra spaces; applied it to create a `clean_text` column and printed a before/after example (review index 10).
6. Plotted a **histogram of review lengths** (word count, 40 bins).
7. Joined all cleaned reviews into one word list to get the **total word count** and **vocabulary size**.
8. Plotted the **top 20 most frequent words** (horizontal bar chart) using `Counter`.
9. Removed English stop words (using the stop list from `CountVectorizer(stop_words="english")`) and plotted the **top 20 words again**.
10. Generated a **word cloud** from the stop-word-filtered words.
11. Extracted the **top 20 bigrams** using `CountVectorizer(ngram_range=(2, 2), stop_words="english", max_features=20)` and displayed them sorted by frequency.

## Results
- **Dataset:** 50,000 rows and 2 columns (`text`, `labels`), perfectly balanced with 25,000 reviews per label.
- **Data quality:** 0 missing values in both columns; **418 duplicate rows** were found (they were detected but not removed in the notebook).
- **Text statistics (across 50,000 reviews):**

  | Statistic | char_count | word_count | unique_word_count | avg_word_length |
  |---|---|---|---|---|
  | mean | 1309.43 | 231.16 | 148.05 | 4.64 |
  | std | 989.73 | 171.34 | 88.84 | 0.34 |
  | min | 32 | 4 | 4 | 1.24 |
  | 25% | 699 | 126 | 91 | 4.42 |
  | 50% | 970 | 173 | 120 | 4.63 |
  | 75% | 1590.25 | 280 | 180 | 4.85 |
  | max | 13704 | 2470 | 1092 | 12.29 |

  The mean word count (231) is well above the median (173), indicating a right-skewed length distribution.
- **Review length histogram:** strongly right-skewed. Most reviews are short, with the peak at roughly 150 to 200 words (about 14,000+ reviews in the tallest bin) and a long tail stretching to about 2,500 words.
- **Cleaning example (review 10):** the original text with `<br />` tags and numbers became plain lowercase letters only (e.g. "30 years ago" became "years ago", "didn't" became "didnt").
- **Corpus size:** **11,259,489** total words and a vocabulary of **214,620** unique words (after cleaning).
- **Top 20 words before stop word removal:** dominated by stop words: `the` (about 650,000 occurrences, read from the bar chart), `and`, `a`, `of`, `to`, `is`, `in`, `it`, `i`, `this`, `that`, `was`, `as`, `with`, `for`, `but`, `on`, `not`. Only `movie` and `film` are content words in this top 20.
- **Top 20 words after stop word removal:** `movie` (about 83,000), `film` (about 74,000), `like`, `just`, `good`, `time`, `really`, `story`, `great`, `bad`, `people`, `dont`, `movies`, `films`, `make`, `way`, `characters`, `think`, `watch`, `seen`. Removing stop words clearly surfaced topic words and opinion words.
- **Word cloud:** `film` and `movie` appear largest, followed by words such as `time`, `story`, `character`, `scene`, `people`, `think`, `good`, and `really`.
- **Top 20 bigrams (with frequency):** `ive seen` (3300), `dont know` (2193), `special effects` (2134), `looks like` (1661), `movie just` (1477), `waste time` (1425), `im sure` (1369), `good movie` (1363), `watch movie` (1296), `dont think` (1296), `look like` (1291), `new york` (1245), `low budget` (1224), `years ago` (1197), `pretty good` (1073), `year old` (1061), `high school` (1059), `main character` (1041), `movie like` (1038), `bad movie` (979).
- **Quirks worth noting:**
  - The cleaning regex removes apostrophes, turning contractions into `dont`, `ive`, `im`. These are not in the stop word list, so they leak into the filtered results (`dont` in the top 20 words; `ive seen`, `dont know`, `im sure`, `dont think` in the bigrams).
  - Bigrams are built after stop words are dropped, so a pair like `movie just` may not have been adjacent in the original sentence.
  - The 418 duplicates were not removed, so they slightly inflate the counts.

## Conclusion
The experiment performed EDA on the IMDb review corpus using text statistics, a review length distribution, frequency plots, a word cloud, and bigram analysis. Stop word removal changed the top word list from function words to topic and opinion words (`movie`, `film`, `good`, `bad`), and the bigrams captured useful phrases like `special effects` and `waste time`. The analysis also exposed preprocessing issues (apostrophe removal, merged words, unremoved duplicates) that would need fixing before sentiment analysis or classification.

## COs Covered
- CO1) Apply various text processing techniques
- CO4) Realize semantics of English language for text processing

## References
- Rizvi College of Engineering, CSDOL7011: Natural Language Processing - Lab Manual, Experiment No. 10.
- Hugging Face `datasets` - `stanfordnlp/imdb` dataset.
- scikit-learn Documentation - `CountVectorizer`.
- WordCloud, Matplotlib, and pandas documentation.