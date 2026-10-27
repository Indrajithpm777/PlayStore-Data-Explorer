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
