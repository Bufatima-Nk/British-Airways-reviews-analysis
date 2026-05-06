# British Airways Customer Sentiment Analysis

> End-to-end NLP pipeline scraping and analyzing **3,000 British Airways customer reviews** from Skytrax. Applied VADER sentiment analysis, LDA topic modeling, and lemmatization to identify key complaint drivers — revealing that Airport Experience is the most negative topic (58.5% negative) while Travel Experience is the most positive (84.3% positive).

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![NLP](https://img.shields.io/badge/NLP-VADER%20%7C%20LDA-orange)
![Reviews](https://img.shields.io/badge/Reviews-3%2C000%20scraped-purple)
![Forage](https://img.shields.io/badge/British%20Airways-Forage%20Virtual%20Internship-005EB8)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## Results

### Sentiment Distribution (3,000 reviews)

| Sentiment | Count | Percentage |
|-----------|:-----:|:----------:|
| Positive | 1,938 | **64.7%** |
| Negative | 1,043 | **34.8%** |
| Neutral | 15 | 0.5% |

Despite a majority of positive reviews, **1 in 3 customers** left a negative review — a significant churn and reputation risk signal.

### Sentiment by Topic (LDA — 5 Topics)

| Topic | Negative % | Neutral % | Positive % | Key Finding |
|-------|:----------:|:---------:|:----------:|-------------|
| **Airport Experience** | **58.5%** | 0.8% | 40.7% | Worst-rated topic — check-in, delays, terminal issues |
| Flight Booking | 51.5% | 1.5% | 47.0% | Booking process and availability problems |
| Customer Service | 44.9% | 0.6% | 54.6% | Mixed; staff interactions split opinions |
| In-Flight Experience | 40.2% | 0.8% | 59.1% | Somewhat positive — seat comfort, food, crew |
| **Travel Experience** | **15.4%** | 0.2% | **84.3%** | Most positive — overall journey satisfaction |

**Key insight:** Airport Experience generates the highest proportion of negative sentiment (58.5%), making pre-flight operations the clearest target for BA service improvement.

### Top 10 Most Frequent Words (after cleaning & lemmatization)

| Rank | Word | Frequency |
|------|------|:---------:|
| 1 | flight | 6,440 |
| 2 | seat | 3,564 |
| 3 | service | 2,458 |
| 4 | food | 1,846 |
| 5 | crew | 1,752 |
| 6 | cabin | 1,629 |
| 7 | time | 1,958 |
| 8 | heathrow | 1,332 |
| 9 | staff | 1,303 |
| 10 | business | 1,300 |

---

## Pipeline

```
Web Scraping (BeautifulSoup)
    ↓ 3,000 reviews from Skytrax (30 pages × 100 reviews)
Text Cleaning
    ↓ Remove verification badges, lowercase, strip punctuation,
    ↓ remove stopwords (sklearn), lemmatize (WordNetLemmatizer)
Sentiment Analysis (VADER)
    ↓ Compound score → Positive / Negative / Neutral
Topic Modeling (LDA — Gensim)
    ↓ 5 topics: Travel, Customer Service, Booking,
    ↓ In-Flight Experience, Airport Experience
Visualization
    ↓ Word cloud, sentiment bar chart, topic-sentiment heatmap,
    ↓ sentiment by review length scatterplot
```

---

## Approach

### 1. Web Scraping
- Scraped 30 pages × 100 reviews = **3,000 total reviews** from [Skytrax British Airways](https://www.airlinequality.com/airline-reviews/british-airways) using `requests` + `BeautifulSoup`
- Targeted `div.text_content` elements
- Removed 3 duplicates → final dataset: **2,997 unique reviews**

### 2. Text Preprocessing
- Stripped verification badges (`✅ Trip Verified |`, `Not Verified |`)
- Lowercased, removed punctuation and numbers
- Removed English stopwords (`sklearn.feature_extraction.text.ENGLISH_STOP_WORDS`)
- Lemmatized tokens with `nltk.stem.WordNetLemmatizer`

### 3. Sentiment Analysis — VADER
- Used `nltk.sentiment.SentimentIntensityAnalyzer` (VADER)
- Compound score thresholds: positive (> 0), negative (< 0), neutral (= 0)
- VADER chosen for its effectiveness on short, informal review text without training data

### 4. Topic Modeling — LDA
- Tokenized cleaned reviews → `corpora.Dictionary` + `corpus` (Bag of Words)
- Trained `gensim.models.LdaModel` with `num_topics=5`
- Assigned dominant topic per review (`argmax` of topic distribution)
- Manually labeled 5 topics based on top words: Travel Experience, Customer Service, Flight Booking, In-Flight Experience, Airport Experience

### 5. Visualizations
- **Word cloud:** Most frequent terms across all 3,000 reviews
- **Sentiment distribution:** Bar chart (positive/negative/neutral counts)
- **Topic-sentiment heatmap:** Proportion of each sentiment within each LDA topic
- **Review length vs. sentiment scatter:** Longer reviews tend toward more extreme scores
- **Word frequency plot:** Top 20 most frequent lemmatized words

---

## Dataset

- **Source:** [Skytrax — British Airways Reviews](https://www.airlinequality.com/airline-reviews/british-airways)
- **Scraped:** 3,000 reviews (pages 1–30, 100 per page, sorted by post date descending)
- **After deduplication:** 2,997 unique reviews
- **Time span:** Reviews from 2015 to present (Skytrax historical data)
- **Saved:** `data/BA_reviews.csv` (raw), `data/selected_BA_reviews.csv` (cleaned + sentiment)

---

## Project Structure

```
British-Airways-reviews-analysis/
│
├── ba_task1.ipynb                  # Full pipeline: scraping → NLP → visualization
├── Key-Insights-Task1-BA.pdf       # Executive summary of findings
├── requirements.txt                # Dependencies
└── README.md
```

---

## How to Run

```bash
git clone https://github.com/Bufatima-Nk/British-Airways-reviews-analysis
cd British-Airways-reviews-analysis
pip install -r requirements.txt
jupyter notebook ba_task1.ipynb
```

> **Note:** The scraping cell will re-collect reviews live from Skytrax. To skip scraping and use saved data, load `data/BA_reviews.csv` directly.

> **NLTK downloads required on first run:**
> ```python
> import nltk
> nltk.download('vader_lexicon')
> nltk.download('wordnet')
> nltk.download('omw-1.4')
> ```

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Web Scraping | `requests`, `BeautifulSoup` |
| Text Processing | `re`, `string`, `nltk` (WordNetLemmatizer), `sklearn` (ENGLISH_STOP_WORDS) |
| Sentiment Analysis | `nltk.sentiment.SentimentIntensityAnalyzer` (VADER) |
| Topic Modeling | `gensim` (LdaModel, corpora.Dictionary) |
| Visualization | `matplotlib`, `seaborn`, `WordCloud` |
| Data | `pandas` |

---

## Key Business Insights

**1. Airport Experience is the biggest pain point.** With 58.5% negative sentiment, pre-flight operations — check-in queues, delays at Heathrow, gate experience — are the most consistent source of dissatisfaction. BA's heavily Heathrow-centric operation makes this a structural risk.

**2. In-flight product is relatively well-regarded.** In-Flight Experience is 59.1% positive, suggesting BA's cabin crew, seat comfort, and food receive generally favorable reviews once passengers are on board.

**3. "Heathrow" appears 1,332 times** — the airport itself is a top-10 most frequent review term, reinforcing the airport experience finding.

**4. Negative reviews are longer on average.** The scatter of sentiment score vs. review length shows that highly negative reviews (compound score < -0.5) tend to be longer — dissatisfied customers write more.

**5. Flight Booking is the second most negative topic (51.5% negative).** Online booking failures, availability issues, and Avios redemption problems drive significant dissatisfaction before travel even begins.

---

## Certificate

Completed as part of the [British Airways Data Science Virtual Experience](https://www.theforage.com/simulations/british-airways/data-science-yqoz) on Forage.

[📄 View Certificate](https://drive.google.com/file/d/1POlhEPHmYFZ3m1dy-NIKrclqnjvBKunQ/view)

---

## Author

**Bufatima N.K.**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-bufatima--n--k-blue?logo=linkedin)](https://linkedin.com/in/bufatima-n-k)
[![GitHub](https://img.shields.io/badge/GitHub-Bufatima--Nk-black?logo=github)](https://github.com/Bufatima-Nk)
