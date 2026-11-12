# Google Play Apps 2026 Analysis

## 📌 Project Overview
The Google Play Store hosts millions of applications, making it challenging to identify the key factors driving high install counts, positive user ratings, and long-term user retention. This project processes and analyzes a 2026 sample of Google Play app listings (`100,013 rows × 19 columns`). 

The primary objective is to transition from intuition-based development and marketing decisions to an evidence-based framework, helping developers, market analysts, and investors spot crowded categories, understand monetisation strategies, and evaluate maintenance patterns.

---

## 🎯 Objectives
* **Profile the Market:** Analyze app counts, games versus non-games distribution, and category/genre concentration.
* **Evaluate Monetisation Strategies:** Compare free vs. paid apps and ads-supported vs. in-app purchase (IAP) models against popularity metrics and ratings.
* **Measure User Satisfaction:** Investigate user scores, rating volumes, and review behavior across different genres and content ratings.
* **Assess App Freshness & Lifespan:** Analyze release timelines and update frequencies to measure developer maintenance health[cite: 1].
* **Identify Market Leaders:** Discover top developers and categories dominating market reach and user engagement[cite: 1].

---

## 📊 Dataset Description
* **Source File:** `google_play_apps_2026_sample (2).csv`[cite: 1]
* **Size:** ~100,013 rows and 19 columns[cite: 1]
* **Key Attributes:**
  * **Identity:** `app_id`, `title`, `developer`, `developer_domain`[cite: 1]
  * **Category:** `genre`, `is_game`, `content_rating`[cite: 1]
  * **Popularity:** `installs` (banded text values like `"1,000+"`), `ratings`, `reviews`[cite: 1]
  * **Quality:** `score` (Average rating from 0 to 5)[cite: 1]
  * **Monetisation:** `free`, `price`, `currency`, `contains_ads`, `offers_iap`[cite: 1]
  * **Timeline:** `released`, `last_updated`[cite: 1]

---

## 🛠️ Tools and Technologies
* **Language:** Python 3.10+[cite: 1]
* **Environment:** Jupyter Notebook[cite: 1]
* **Libraries:**
  * `pandas` & `NumPy` for data cleaning, transformation, and KPI calculation[cite: 1]
  * `matplotlib` & `seaborn` for exploratory data visualization (histograms, box plots, correlation heatmaps, bar charts)[cite: 1]

---

## 🔄 Project Workflow
1. **Data Loading & Validation:** Loaded the raw CSV file safely using Pandas, checking data types, memory usage, and initial row dimensions.
2. **Data Preprocessing & Cleaning:**
   * Handled missing values and removed duplicate entries while tracking drop counts.
   * Converted text-banded `installs` data into clean numeric values (using lower bounds of bands).
   * Standardized text fields for `genre` and `content_rating`.
   * Engineered derived features: app age (years), days since last update, a binary `stale` flag (no updates in 12+ months), release year, and install-tier bands.
   * Addressed multi-currency price distributions and outliers.
3. **Exploratory Data Analysis (EDA) & KPI Calculation:** Computed key performance metrics covering market composition, median installs, pricing structures, monetisation overlaps, and maintenance health.
4. **Data Visualization:** Generated publication-quality charts and plots with clear labeling to illustrate distributions and segment comparisons[cite: 1].

---

## 📈 Key Analysis & Business Questions Explored
* Which genres command the highest volume of installs versus user ratings?
* Do ads-supported or IAP-based models correlate better with higher user scores?
* What percentage of apps on the Google Play Store suffer from lack of maintenance (stale for 12+ months)?
* How does app pricing affect user download behavior across paid and free segments?

---
# Data Cleaning & Preprocessing

This document outlines the data cleaning, preprocessing, and feature engineering steps executed on the Google Play Store dataset ($N = 100,013$ apps) inside the Jupyter Notebook. Each section details the analytical action, the technical method or function used, and the underlying rationale.

---

### 1. Imputing Missing Performance Metrics (`score`)

* **Action:** Resolved missing values in the `score` column without dropping records, preserving the full sample size.
* **Method/Function Used:** Pandas `fillna()` combined with `groupby('genre')['score'].transform('median')`.
* **Rationale:** Niche or newly listed apps often lack score metrics. Grouping by application `genre` and imputing with the group median ensures that the substitute values align with category-specific rating benchmarks rather than a global average that might distort genre characteristics.

### 2. Dropping Redundant Metadata (`fliar_profile_url`)

* **Action:** Removed the `fliar_profile_url` column entirely from the DataFrame.
* **Method/Function Used:** Pandas `drop(columns='fliar_profile_url', inplace=True)`.
* **Rationale:** The column only contained valid data for roughly 30% of records and served solely as an external URL reference, offering no analytical utility toward core evaluation objectives.

### 3. Cleaning and Casting Adoption Scale (`installs`)

* **Action:** Standardized string values and converted the `installs` column into a discrete integer data type.
* **Method/Function Used:** Pandas `.str.replace('+', '', regex=False)`, `.str.replace(',', '', regex=False)`, and `.astype(int)`.
* **Rationale:** Raw install metrics included trailing plus signs and comma separators (e.g., `10,000,000+`), which restricted them to string object types. Removing these formatting characters enables proper numerical aggregation and sorting.

### 4. Groupby Median Imputation for Adoption Metrics (`ratings` & `reviews`)

* **Action:** Imputed missing values for `ratings` and `reviews` using adoption tier metrics, then cast them to integers (`int64`).
* **Method/Function Used:** Pandas `fillna()` with `groupby('installs')['ratings'].transform('median')` and `groupby('installs')['reviews'].transform('median')`, followed by `.astype(int)`.
* **Rationale:** User engagement metrics scale proportionally with download volume. Grouping missing entries by their `installs` tier and applying the median maintains realistic scaling relationships across adoption tiers.

### 5. Imputing Pricing and Developer Domains

* **Action:** Filled missing values in `price` and `developer_domain`.
* **Method/Function Used:** Pandas `fillna()` grouped by `genre` median for `price`, and a static string replacement (`'No-Domain'`) for `developer_domain`.
* **Rationale:** Resolving the 13 missing price values using category-specific medians prevents distortion in monetization analyses, while placeholder strings for missing domains prevent null-pointer exclusions during publisher analysis.

### 6. Standardizing Column Headers and Text Fields

* **Action:** Stripped trailing/leading whitespace from all column names and purged non-ASCII special characters from app titles.
* **Method/Function Used:** Pandas `.columns.str.strip()` and `.str.replace(r"[^\x00-\x7F]+", "", regex=True)`.
* **Rationale:** Eliminates syntax errors during programmatic column referencing and standardizes text display by stripping corrupted or non-standard characters from titles.

### 7. Datetime Parsing and Temporal Null Handling (`released` & `last_updated`)

* **Action:** Converted release and update date strings into standardized datetime objects (`datetime64[us]`).
* **Method/Function Used:** Pandas `pd.to_datetime()`.
* **Rationale:** Automatically flags unparseable or missing release dates as `NaT` (Not a Time) without dropping rows, preserving historical records for alternative analyses where release timestamps are optional.

### 8. Feature Engineering: Temporal Metrics (`released_year`, `last_updated_year`, `app_age`)

* **Action:** Extracted temporal components and calculated application lifespan using nullable integer architecture.
* **Method/Function Used:** Pandas `.dt.year` accessor and `.astype('Int64')`.
* **Rationale:** Utilizing Pandas' nullable integer extension type (`Int64` with a capital `I`) allows calculations involving `NaT` values to propagate cleanly as `<NA>` rather than throwing type-casting errors.

### 9. Feature Engineering: Custom Monetization Categorization (`monetization_category`)

* **Action:** Built a composite classification feature mapping business and revenue models.
* **Method/Function Used:** Custom Python function evaluated row-wise via `apps.apply(categorize_monetization, axis=1)`.
* **Rationale:** Standard boolean columns (`free`, `contains_ads`, `offers_iap`) require multi-variable logic to understand actual app business models. This function categorizes apps into mutually exclusive groups: `Paid`, `Free with Ads and IAP`, `Free with Ads`, `Free with IAP`, or `Free without Ads or IAP`.

---

# 📊 Play Store App Demographics and Technology: Exploratory Data Analysis (EDA)

An in-depth Exploratory Data Analysis (EDA) of **100,013 mobile applications** sourced from the Google Play Store sample dataset. This document details the statistical distributions, univariate trends, bivariate relationships, and multivariate interactions uncovered during the evaluation phase.

---

## 📈 Statistical Summary & Dataset Overview
Following data preprocessing and feature engineering, the dataset encompasses **100,013 rows and 22 structured features**:
* **Catalog Scale:** Install volumes span from a minimum of $1,000$ to over $10,000,000,000$, with a mean of ~651,966 installs.
* **Pricing Dynamics:** While the median and 75th percentile for price sit at $0.00$ (reflecting a predominantly free ecosystem), the maximum price reaches **$294.99**, capturing premium utility and niche professional software.
* **User Sentiment Stability:** App scores range from $1.0$ to $5.0$, with a median of **4.20** and a mean of **4.15**, proving that active listings on the Play Store generally maintain positive user satisfaction.
* **App Age & Lifespan:** The average app age in the sample is approximately **4 years**, with releases spanning from 2010 up to mid-2026.
* **Genre Frequency:** *Education* emerges as the most frequently occurring genre (10,438 entries) in this specific sample, while USD serves as the universal currency.

---

## 🔍 Exploratory Data Analysis Breakdown

### 1. Univariate Analysis (Single-Variable Exploration)
* **App Age Distribution:** Heavily concentrated toward the lower end of the age spectrum, peaking sharply between **0 to 3 years old**. This steep drop-off in older brackets highlights high developer turnover—many apps are frequently updated as new versions or abandoned/delisted if they fail to maintain active user engagement.
* **Score Distribution:** Exhibits a left-skewed distribution clustered heavily between **4.0 and 4.6**. A noticeable sharp peak appears around 4.0–4.3 due to genre-level median imputations for missing values.
* **Releases Over Time:** App releases grew steadily through the late 2010s, accelerating sharply post-2020 to reach peak annual volumes (~10,000+ apps/year) during 2024–2025 before dropping in 2026 due to the mid-year dataset snapshot cutoff.
* **Game vs. Non-Game Proportion:** Non-game applications dominate catalog volume at **85.3%**, while mobile games account for 14.7%. Despite smaller volume, gaming genres capture half of the top spots for average installs.
* **Monetization Breakdown:** Over **98%** of apps are free. Paid apps make up a niche 1.5% minority, while ad-supported models (*Free with Ads*) serve as the primary monetization vehicle.

### 2. Bivariate Analysis (Two-Variable Relationships)
* **Top Genres by Average Ratings & Installs:** 
  * *Social* and *Music* lead average user adoption (~8.5M and ~5.6M average installs respectively), driven by massive flagship applications like Facebook.
  * *Action*, *News & Magazines*, and *Communication* form a strong second tier.
* **Developer Ecosystem Analysis:** While indie developers (*Individual developer*) contribute over 6,000 catalog entries, global download volume and engagement are heavily led by tech giants like **Meta Platforms, Inc.**
* **Independent Developer Performance:** Solo developers successfully target high-demand utility categories (e.g., *Tools* like File Manager achieving 1B+ installs), though textual review counts drop off compared to casual star ratings.
* **Content Ratings & Scores:** *Everyone* and *Teen* categories display the widest spread of score outliers due to housing the vast majority of applications.

### 3. Multivariate Analysis (Multi-Variable Interactions)
* **Monetization Across Top Genres:** *Education* dominates the *Free without Ads and IAP* tier (funneling users to premium content), whereas *Music & Audio* predominantly utilizes ad-supported free models with subscription upsells.
* **Installs vs. Ratings (Log-Log Scale):** Confirms a strong, predictable scaling relationship between adoption volume and user feedback across top genres.
* **Correlation Heatmap Findings:**
  * **Strong Positive Correlation:** Installs correlate strongly with ratings ($r = 0.96$) and reviews ($r = 0.92$).
  * **Strong Negative Correlation:** App age and release year ($r = -0.88$), as older release years naturally yield higher app ages.
  * **Near-Zero Correlation:** Price shows zero correlation with scores ($r \approx 0.00$), as the overwhelming majority of successful apps operate on free or ad-supported models.

---

## 🎯 Executive Summary & Key Takeaways

1. **The Free-To-Download Economy Rules:** Over 98% of apps are free, relying on ad-supported models to maintain high acquisition while generating revenue.
2. **Indie Volume vs. Enterprise Scale:** The Play Store ecosystem relies on a duality: millions of independent developers supplying lightweight utility and education apps, versus a handful of tech giants (like Meta) capturing the bulk of global user engagement.
3. **The Illusion of Scale vs. Engagement:** While total installs correlate heavily with star ratings ($r = 0.96$), text review volumes drop sharply for independent utility apps—indicating users frequently tap star ratings for permissions/prompts without leaving qualitative feedback unless an app is exceptional or broken.

---



