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
## 📂 Project Structure
```text
google-play-apps-analysis/
│
├── data/
│   └── google_play_apps_2026_sample (2).csv
│
├── notebooks/
│   └── google_play_apps_analysis.ipynb
│
├── outputs/
│   └── figures/          # Generated charts and graphs
│
├── requirements.txt      # Python dependencies
└── README.md
