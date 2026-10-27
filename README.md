# PlayStore-Data-Explorer
# Google Play Store Apps (2026) Analysis 📱📊

A comprehensive data analysis repository exploring Google Play Store applications based on the **2026 dataset** (`filiarbi/google-play-apps-2026`). This project investigates app metrics, user engagement patterns, developer distributions, pricing strategies, and rating drivers.

---

## 🚀 Project Overview

The Google Play Store ecosystem hosts millions of applications spanning productivity, games, social networking, utilities, and more. This project cleans, analyzes, and extracts key insights from a comprehensive dataset of over 100,000 Android applications to answer crucial questions:

* What are the most dominant app categories and genres?
* How do monetization models (Free vs. Paid, IAP, Ads) impact user ratings and installs?
* What correlations exist between app ratings, review counts, and total installation volumes?
* Who are the leading developers in terms of portfolio size and reach?

---

## 🗂️ Dataset Description

The dataset (`google_play_apps_2026_sample (2).csv`) includes **100,013 records** across **19 columns**:

| Column Name | Data Type | Description |
| --- | --- | --- |
| `app_id` | `object` | Unique package name identifier (e.g., `com.facebook.katana`) |
| `title` | `object` | Display name of the application |
| `developer` | `object` | Name of the publisher/developer |
| `developer_domain` | `object` | Official website domain of the developer |
| `genre` | `object` | Primary category/genre of the app |
| `is_game` | `bool` | Flag indicating whether the app is a game (`True`/`False`) |
| `installs` | `object` | Estimated installation tier (e.g., `10,000,000,000+`) |
| `score` | `float` | Average user rating score (out of 5.0) |
| `ratings` | `float` | Total number of user ratings submitted |
| `reviews` | `float` | Total volume of user reviews |
| `free` | `bool` | Whether the app is available for free |
| `price` | `float` | Upfront purchase price in USD |
| `currency` | `object` | Currency code (`USD`) |
| `contains_ads` | `bool` | Presence of in-app advertisements |
| `offers_iap` | `bool` | Presence of In-App Purchases (IAP) |
| `content_rating` | `object` | ESRB / Content maturity rating (e.g., Teen, Everyone) |
| `released` | `object` | Initial release date |
| `last_updated` | `object` | Timestamp of the most recent update |
| `fliar_profile_url` | `object` | Source publisher profile link |

---

## 🛠️ Tech Stack & Requirements

* **Language:** Python 3.8+
* **Data Manipulation & Analysis:** Pandas, NumPy
* **Visualization:** Matplotlib, Seaborn
* **Environment:** Jupyter Notebook / Google Colab

To run the analysis scripts locally, install the required dependencies:

```bash
pip install pandas numpy matplotlib seaborn

```

---

## ⚙️ Key Analysis Workflow

1. **Data Cleaning & Preprocessing:**
* Handled missing values across ratings, reviews, and developer domains.
* Parsed installation string tiers into numerical formats for quantitative comparisons.
* Standardized date fields for temporal trend analysis.


2. **Exploratory Data Analysis (EDA):**
* **Category & Genre Breakdown:** Identified the proportion of games vs. productivity/utility applications.
* **Monetization Strategy:** Analyzed the distribution of free vs. paid apps alongside In-App Purchase (IAP) integration and ad monetization.
* **Rating & Review Distribution:** Evaluated skewed rating distributions and user engagement depth.



---

## 📈 Key Insights & Findings

* **Free-to-Play Dominance:** The vast majority of applications are free upfront, relying heavily on **In-App Purchases (IAP)** and **Ads** for monetization.
* **Rating Concentration:** Most apps cluster around high ratings (4.0 - 4.8), though applications with massive review volumes exhibit stable, realistic averages due to Law of Large Numbers.
* **Category Splits:** Non-game utility and communication tools drive massive install brackets (billions of downloads), whereas games dominate sheer title volume and diversity.

---

## 📂 Repository Structure

```text
├── data/
│   └── google_play_apps_2026_sample.csv    # Raw dataset
├── notebooks/
│   └── play_store_eda.ipynb                # Jupyter notebook with full analysis & charts
├── scripts/
│   └── data_preprocessing.py               # Data cleaning pipelines
├── README.md                               # Project documentation
└── requirements.txt                        # Python dependencies

```

---

## 🤝 Contributing

Contributions, feature requests, and bug reports are welcome! Feel free to open an issue or submit a pull request.

1. Fork the Project (`[https://github.com/your-username/google-play-apps-2026/fork](https://github.com/your-username/google-play-apps-2026/fork)`)
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

Distributed under the **MIT License**. See `LICENSE` for more information.
