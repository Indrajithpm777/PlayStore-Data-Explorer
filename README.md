# 📱 Play Store App Demographics & Technology: Category Adoption and User Base

> An end-to-end exploratory data analysis (EDA) of **100,013 Google Play apps** (2026 sample): what drives installs, ratings, monetisation choices and developer success.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/matplotlib-visualisation-11557c)
![Seaborn](https://img.shields.io/badge/seaborn-statistical%20plots-4c72b0)
![Plotly](https://img.shields.io/badge/plotly-interactive%20charts-3F4F75?logo=plotly&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-notebook-F37626?logo=jupyter&logoColor=white)

**Domain:** App-Store Analytics & Technology  |  **Type:** Exploratory Data Analysis  |  **Records:** 100,013  |  **Features:** 19 raw → 22 after feature engineering

---

## 📑 Table of Contents

1. [Project Overview](#-project-overview)
2. [Business Problem](#-business-problem)
3. [Objectives](#-objectives)
4. [Key Findings at a Glance](#-key-findings-at-a-glance)
5. [Dataset](#-dataset)
6. [Tools & Technologies](#-tools--technologies)
7. [Project Workflow](#-project-workflow)
8. [Data Cleaning & Preprocessing](#-data-cleaning--preprocessing)
9. [Exploratory Data Analysis](#-exploratory-data-analysis)
10. [Business Insights & Recommendations](#-business-insights--recommendations)
11. [Scope, Assumptions & Limitations](#-scope-assumptions--limitations)


---

## 🔭 Project Overview

The mobile app market is hyper-competitive. Developers, product managers and analysts struggle to tell **which genres have real demand**, **how monetisation choices (ads, in-app purchases, paid) relate to engagement**, and **who actually captures users**.

This project analyses a sample of **100,013 Google Play listings** to answer those questions. It covers the full analytics workflow: data inspection, cleaning, feature engineering, and univariate, bivariate and multivariate analysis, finishing with a business-focused summary.

---

## 💼 Business Problem

Understanding what users really want in a crowded app market is difficult. Genre interest, release trends, engagement levels, ratings and reviews are the signals that matter, but they only become useful once the data is cleaned, structured and compared properly.

**Who benefits from this analysis**

| Audience | How they can use it |
|---|---|
| App developers | Pick genres and monetisation models with proven demand |
| Product managers | Align update cycles and features with market behaviour |
| Market researchers / analysts | Benchmark categories, developers and adoption tiers |
| Investors | Spot crowded vs. under-served segments |

---

## 🎯 Objectives

| # | Question | Approach |
|---|---|---|
| 1 | Which genres dominate in volume and downloads? | `groupby('genre')` on average installs |
| 2 | What patterns exist between engagement metrics? | Compare installs, ratings, reviews, score and content rating |
| 3 | Which developers build the most successful apps? | Total installs per developer, plus a deep dive on Meta and individual developers |
| 4 | What is the real share of games vs. non-games? | Proportion of `is_game` |
| 5 | How does monetisation vary across genres? | Paid / free / ads / in-app purchase (IAP) breakdown |

---

## ⚡ Key Findings at a Glance

| Finding | Result |
|---|---|
| Free apps | **98.5%** of the catalogue (98,537 free vs 1,476 paid) |
| Games vs non-games | **85.3%** non-games, **14.7%** games |
| Median user score | **4.20** (mean 4.15) |
| Top genres by average installs | **Social** (~8.5M) and **Music** (~5.6M) |
| Most common genre | **Education** (10,438 apps) |
| Developers | **37,480** unique developers; the "Individual developer" label alone covers **6,022** apps |
| Installs vs ratings | Strong positive correlation, **r = 0.96** |
| Installs vs reviews | Strong positive correlation, **r = 0.92** |
| Price vs score | Essentially none, **r ≈ 0.00** |
| Largest developer by total installs | **Meta Platforms, Inc.** (12 apps in this sample) |

---

## 🗂️ Dataset

| Property | Detail |
|---|---|
| **Source** | [Kaggle: Google Play Apps 2026 (starter)](https://www.kaggle.com/code/fliarbi/google-play-apps-2026-starter/input?select=google_play_apps_2026_sample.csv) |
| **File** | `google_play_apps_2026_sample.csv` |
| **Format** | CSV |
| **Size** | 100,013 rows × 19 columns (22 after feature engineering, 1 dropped) |
| **One row =** | One mobile application listed on Google Play, with its usage stats, metadata, monetisation model and publisher |
| **Primary key** | `app_id` (verified unique) |

### Data dictionary

| Group | Column | Description |
|---|---|---|
| Identity | `app_id` | Unique package name (primary key) |
| | `title` | App name |
| | `developer` | Publisher name |
| | `developer_domain` | Publisher website domain |
| Category | `genre` | One of 48 genres |
| | `is_game` | `True` if the app is a game |
| | `content_rating` | Age-suitability rating (5 levels) |
| Adoption | `installs` | Install band as text, e.g. `"10,000+"` |
| | `released` | Release date |
| | `last_updated` | Date of last update |
| Engagement | `score` | Average user rating (1.0 to 5.0) |
| | `ratings` | Number of star ratings |
| | `reviews` | Number of written reviews |
| Monetisation | `free` | `True` if free to download |
| | `price` | Price in USD (the only currency in the data) |
| | `contains_ads` | `True` if the app shows ads |
| | `offers_iap` | `True` if it offers in-app purchases |
| Dropped | `fliar_profile_url` | External link, removed during cleaning |

### Engineered features (added in this project)

| Column | How it is built |
|---|---|
| `released_year` | Year taken from `released` |
| `last_updated_year` | Year taken from `last_updated` |
| `app_age` | `last_updated_year - released_year` |
| `monetization_category` | One label per app: `Paid`, `Free with Ads and IAP`, `Free with Ads`, `Free with IAP` or `Free without Ads and IAP` |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python 3.10+ | Core language |
| Jupyter Notebook | Interactive analysis and documentation |
| pandas, NumPy | Cleaning, imputation, feature engineering, aggregation |
| matplotlib, seaborn | Static charts (bar, pie, histogram, box plot, heatmap, scatter) |
| plotly express | Interactive histogram and bubble chart |

---

## 🔄 Project Workflow

```
 Load CSV ─► Inspect ─► Clean & impute ─► Engineer features ─► Univariate EDA
                                                                      │
 Executive summary ◄─ Multivariate EDA ◄─ Bivariate EDA ◄────────────┘
```

---

## 🧹 Data Cleaning & Preprocessing

### Step 0: Inspect the raw data

```python
import pandas as pd

apps = pd.read_csv("google_play_apps_2026_sample.csv")

apps.shape                  # (100013, 19)
apps.info()                 # data types + non-null counts
apps.duplicated().sum()     # 0  -> no duplicate rows
apps['app_id'].is_unique    # True -> app_id is a valid primary key
apps.isna().sum()           # missing values per column
```

### Missing values found

| Column | Missing | Share | Treatment |
|---|---:|---:|---|
| `score` | 68,258 | 68.2% | Median of the same `genre` |
| `ratings` | 68,258 | 68.2% | Median of the same `installs` tier |
| `reviews` | 68,258 | 68.2% | Median of the same `installs` tier |
| `developer_domain` | 36,486 | 36.5% | Filled with `'No-Domain'` |
| `released` | 17,467 | 17.5% | Kept as `NaT` (rows not dropped) |
| `price` | 13 | 0.01% | Median of the same `genre` |
| `fliar_profile_url` | 69,638 | 69.6% | Column dropped |

> Nothing was deleted: every treatment preserves all **100,013 rows**.

### Step-by-step with code

**1. Fill missing `score` with the genre median.**
A missing score for a Puzzle game should look like other Puzzle games, not like the whole store.

```python
apps['score'] = apps['score'].fillna(
    apps.groupby('genre')['score'].transform('median')
)
```

**2. Drop `fliar_profile_url`.**
About 70% empty and it is only a link, so it adds nothing to the objectives.

```python
apps.drop(columns='fliar_profile_url', inplace=True)
```

**3. Convert `installs` from text to numbers.**
Values like `"10,000,000+"` are strings; numbers are needed to sort, average and plot.

```python
apps['installs'] = (
    apps['installs']
    .str.replace('+', '', regex=False)    # "10,000,000+" -> "10,000,000"
    .str.replace(',', '', regex=False)    # "10,000,000"  -> "10000000"
)
apps['installs'] = apps['installs'].astype(int)
```

**4. Fill `ratings` and `reviews` using the install tier.**
Apps with 1M installs collect far more ratings than apps with 1K, so each gap is filled from apps of similar size.

```python
apps['ratings'] = apps['ratings'].fillna(
    apps.groupby('installs')['ratings'].transform('median')
).astype(int)

apps['reviews'] = apps['reviews'].fillna(
    apps.groupby('installs')['reviews'].transform('median')
).astype(int)
```

**5. Fill `price` and `developer_domain`.**

```python
# 13 missing prices -> median price of the same genre
apps['price'] = apps['price'].fillna(
    apps.groupby('genre')['price'].transform('median')
)

# Missing domain -> explicit placeholder so these rows stay in publisher analysis
apps['developer_domain'] = apps['developer_domain'].fillna('No-Domain')
```

**6. Standardise column names and titles.**

```python
apps.columns = apps.columns.str.strip()      # remove stray spaces in headers

# remove non-ASCII junk characters from app titles
apps['title'] = (
    apps['title'].str.replace(r"[^\x00-\x7F]+", "", regex=True).str.strip()
)
```

**7. Convert dates to real datetimes.**
Unparseable or missing release dates become `NaT` instead of crashing or forcing a row drop.

```python
apps['released']     = pd.to_datetime(apps['released'])
apps['last_updated'] = pd.to_datetime(apps['last_updated'])
```

**8. Create time-based features.**
`Int64` (capital I) is pandas' nullable integer, so apps with no release date stay `<NA>` instead of raising errors.

```python
apps['released_year']     = apps['released'].dt.year.astype('Int64')
apps['last_updated_year'] = apps['last_updated'].dt.year
apps['app_age']           = apps['last_updated_year'] - apps['released_year']
```

**9. Build one monetisation label from three True/False columns.**

```python
def categorize_monetization(row):
    if not row['free']:
        return 'Paid'
    if row['contains_ads'] and row['offers_iap']:
        return 'Free with Ads and IAP'
    if row['contains_ads']:
        return 'Free with Ads'
    if row['offers_iap']:
        return 'Free with IAP'
    return 'Free without Ads and IAP'

apps['monetization_category'] = apps.apply(categorize_monetization, axis=1)
```

### Final check after cleaning

```python
apps.info()        # 22 columns; the only nulls left are released, released_year and app_age (17,467 apps with no release date)
apps['monetization_category'].value_counts()
```

---

## 📊 Exploratory Data Analysis

**Summary statistics (after cleaning)**

| Metric | Value |
|---|---|
| Installs range | 1,000 to 10,000,000,000 (median 5,000; mean ≈ 651,966) |
| Score range | 1.0 to 5.0 (median 4.20, mean 4.15) |
| Price | Median and 75th percentile = $0.00; maximum = $294.99 |
| Average app age | About 4 years; releases span 2010 to mid-2026 |
| Unique genres / developers / titles | 48 / 37,480 / 91,254 |

---

### 🔹 Univariate Analysis (one variable at a time)

#### App age distribution
![App age distribution](images/01_app_age_distribution.png)

Most apps are **0 to 3 years old**, with a sharp drop-off beyond that. This points to high developer turnover: apps are replaced by newer versions or removed if they fail to hold users.

#### Score distribution
![Score distribution](images/02_score_distribution.png)

The distribution is left-skewed, with most scores between **4.0 and 4.6**. The sharp peak around 4.0 to 4.3 is partly an effect of filling the 68% missing scores with genre medians.

#### Releases per year
![Releases per year](images/03_releases_per_year.png)

Releases grew steadily through the 2010s and accelerated after 2020, peaking at roughly **10,000+ apps per year in 2024 to 2025**. The 2026 dip is not a slowdown: the snapshot ends in mid-2026, so only half the year is captured.

#### Games vs non-games
![Game vs non-game](images/04_game_vs_nongame.png)

Non-game apps make up **85.3%** of the sample and games **14.7%**. Even so, gaming genres take half of the top 10 spots for average installs per app.

#### Monetisation breakdown
![Monetisation breakdown](images/05_monetization_breakdown.png)

Free apps rule the store (**98.5%**) and paid apps are a **1.5%** niche. Among free apps, ad-supported apps form the second-largest group, making ads the most common way to earn while keeping the download free.

---

### 🔹 Bivariate Analysis (two variables)

#### Top 10 genres by average ratings
![Top genres by average ratings](images/06_top_genres_avg_ratings.png)

**Social** leads with an average of about 100,000+ ratings per app. Communication, Trivia and Action follow as the most actively rated categories.

#### Top 10 genres by average installs
![Top genres by average installs](images/07_top_genres_avg_installs.png)

- **Social (~8.5M)** and **Music (~5.6M)** lead average adoption, pulled up by flagship apps such as Facebook.
- **Action, News & Magazines and Communication** form a second tier at roughly 2.6M to 3.0M average installs.
- Game genres (Action, Arcade, Simulation, Board, Trivia) make up half of the top 10.

#### Top 10 developers by total installs
![Top developers](images/08_top_developers_installs.png)

**Meta Platforms, Inc.** leads total installs with only **12 apps** in the sample. Its apps sit mainly in Social, Communication and Business.

#### Individual developers: top genres
![Individual developers top genres](images/09_indie_top_genres.png)

"Individual developer" is the most frequent publisher label (**6,022 apps**, against 37,480 unique developers). Solo creators concentrate on **Tools**, serving lightweight, specific user needs.

#### Individual developers: top apps
![Individual developers top apps](images/10_indie_top_apps.png)

| App | Genre | Installs | Score | Ratings | Reviews |
|---|---|---:|---:|---:|---:|
| File Manager | Tools | 1,000,000,000 | 4.40 | 192,119 | 407 |
| My Health | Health & Fitness | 100,000,000 | 3.91 | 49,391 | 89 |
| Music Piano 7: Rush Song Games | Music | 50,000,000 | 4.73 | 397,226 | 2,560 |
| Camera for Android | Photography | 50,000,000 | 4.78 | 363,148 | 5,382 |
| App Lock - Fingerprint Pattern | Tools | 50,000,000 | 4.20 | 163,387 | 1,577 |

The top solo-developer app has **1B+ installs but only 407 written reviews**: users tap stars readily but rarely write text for utility apps. Photography and Music apps show the highest satisfaction.

#### Score by content rating
![Score by content rating](images/11_score_by_content_rating.png)

The **Everyone** and **Teen** groups show the widest spread of low-score outliers, simply because they hold the vast majority of apps.

---

### 🔹 Multivariate Analysis (three or more variables)

#### App age vs score, by monetisation (bubble size = ratings)
![App age vs score](images/12_app_age_vs_score.png)

*Static snapshot of an interactive plotly chart (random sample of 5,000 apps).*

- The biggest bubbles (high rating volume) sit mostly in the **0 to 5 year** range.
- Older apps (5 to 15 years) use a wider mix of monetisation models.
- Scores stay between roughly **4.0 and 5.0** whether an app is new or a veteran.

#### Monetisation across the top 5 genres
![Monetisation across top genres](images/13_monetization_top5_genres.png)

- "Free without Ads and IAP" and "Free with Ads" dominate in every major genre.
- **Education** leads the "Free without Ads and IAP" group, a pattern consistent with funnelling learners to paid courses.
- **Music & Audio** leans on ad-supported free apps with subscription upgrades.

#### Installs vs ratings (log-log)
![Installs vs ratings](images/14_installs_vs_ratings.png)

Feedback volume scales predictably with adoption across genres. High-reach social and utility apps occupy the top tiers, while specialised and educational apps sit in lower-volume tiers.

#### Correlation heatmap
![Correlation heatmap](images/15_correlation_heatmap.png)

| Pair | Correlation | Reading |
|---|---:|---|
| installs ↔ ratings | **0.96** | Strong positive |
| installs ↔ reviews | **0.92** | Strong positive |
| app_age ↔ released_year | **-0.88** | Expected: app age is derived from release year |
| price ↔ score | **≈ 0.00** | A higher price does not mean a better rating |

---

## 💡 Business Insights & Recommendations

1. **Free-to-download is the norm.** With 98.5% of apps free, charging upfront is an uphill choice. Ads (and IAP) are the realistic monetisation path.
2. **Two ecosystems coexist.** Thousands of independent developers serve niche utility needs, while a handful of tech giants such as Meta capture most total installs. Indie developers should compete on focused utility, not breadth.
3. **Genre choice shapes reach.** Social and Music lead average installs, but those averages are pulled up by a few flagship apps. Education is the most crowded genre, so differentiation matters.
4. **Quality is stable, volume is not.** Scores stay around 4.0 to 4.6 across ages and genres; installs and rating volume vary by orders of magnitude.
5. **Don't read stars as depth.** Large utility apps get many star ratings but few written reviews, so review text is a weaker signal than rating counts.
6. **Price is not a quality signal.** Price has no relationship with score here.

---

## ⚠️ Scope,Assumptions & Limitations

**In scope:** the 100,013 listings and 19 fields in the single file, analysed descriptively.
**Out of scope:** revenue, downloads over time, user demographics, review text, rankings, and anything outside the file.

**Assumptions:** each row is a distinct, currently listed app; install bands and scores are accurate as scraped.

**Limitations**

- **Sample data.** The file is a sample, so conclusions apply to this sample, not the entire Play Store.
- **Installs are bands, not exact counts.** The lower bound of each band is used, which limits precision.
- **Heavy imputation.** 68% of `score`, `ratings` and `reviews` were missing and filled with medians. This creates the visible spike near 4.0 to 4.3 in scores. Because ratings and reviews were filled using the `installs` tier, part of the strong installs-vs-ratings correlation (0.96 / 0.92) may reflect that imputation, so treat those values as indicative. Re-checking on only the apps with real ratings would be a good validation.
- **Missing release dates.** 17,467 apps have no release date, so age-based analysis covers only apps with a date.
- **Single snapshot.** The data cannot show how an app changes over time.
- **Correlation is not causation.** For example, ads and ratings may move together without one causing the other.
- **Small-rating scores are unreliable.** Scores based on very few ratings should be read cautiously.

---

