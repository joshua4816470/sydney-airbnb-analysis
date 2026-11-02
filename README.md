<div align="center">

# 🏙️ Sydney Airbnb Analysis

### Short Stay Rental Market Analytics

*One clean dataset. Two questions answered together: **where does the Sydney short stay market concentrate and what does it cost**, and **which hosts and suburbs set the standard for quality?***

<br>

<img src="https://img.shields.io/badge/POWER_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power BI">
<img src="https://img.shields.io/badge/PYTHON-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
<img src="https://img.shields.io/badge/PANDAS-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
<img src="https://img.shields.io/badge/JUPYTER-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter">
<img src="https://img.shields.io/badge/CSV-SOURCE_DATA-4D4D4D?style=for-the-badge" alt="CSV">

<br><br>

<img src="https://img.shields.io/badge/status-in%20development-orange?style=flat-square" alt="Status">
<img src="https://img.shields.io/badge/data-Inside%20Airbnb%20Sydney-0A84FF?style=flat-square" alt="Data">
<img src="https://img.shields.io/badge/raw%20listings-20%2C573-lightgrey?style=flat-square" alt="Raw listings">
<img src="https://img.shields.io/badge/active%20listings-13%2C055-2EA44F?style=flat-square" alt="Active listings">

</div>

<br>

## 📑 Table of Contents

- [Overview](#-overview)
- [Business Problem](#-business-problem)
- [Key Findings](#-key-findings)
- [Dashboard](#-dashboard)
- [Methodology](#-methodology)
- [Dataset](#-dataset)
- [Feature Engineering](#-feature-engineering)
- [Repository Structure](#-repository-structure)
- [How to Run](#-how-to-run)
- [Tech Stack](#-tech-stack)
- [Roadmap](#-roadmap)
- [Author](#-author)

<br>

## 🔭 Overview

Short stay rental data is rich but messy. The raw Sydney listings file contains more than 70 columns, many of them empty, duplicated or irrelevant, along with prices stored as text, inconsistent bathroom and bedroom fields, and a large share of listings that no longer attract guests.

This project turns that raw file of **20,573 listings** into a clean, analysis ready dataset of approximately **13,055 active short stay rentals**, then visualises the market in Power BI. Every cleaning decision is documented inside the notebook so the process is transparent, repeatable and easy to audit.

<br>

## 🎯 Business Problem

Investors, hosts and analysts need a reliable view of the Sydney short stay market, but raw listing data cannot be trusted as is. Dormant listings, long stay rentals and extreme prices distort every average. This project answers:

- Which suburbs have the highest concentration of active listings?
- Where are the most expensive and most affordable suburbs by median nightly price?
- How is the market split between individual hosts, small operators and commercial operators?
- Which suburbs have the highest share of Superhosts?
- How do room type, amenities and host size relate to price?

<br>

## 💡 Key Findings

| Metric | Value |
|:---|:---|
| Raw listings | 20,573 |
| Active short stay listings retained | approximately 13,055 (63 percent) |
| Unique hosts | 5,969 |
| Median nightly price | 308 AUD |
| Superhost share | 42.41 percent |
| Listings with no review in 12 months | 28.6 percent |
| Listings requiring stays of 28 nights or more | 14.5 percent |
| Listings run by hosts with 5 or more properties | 45 percent |
| Missing bathroom values, before and after repair | 27.8 percent to about 0.1 percent |

**Headline insights from the dashboard**

- Sydney, Waverley and Randwick hold the largest number of listings.
- Pittwater, Manly, Woollahra and Mosman lead on median nightly price.
- Commercial hosts operate close to half of all listings, followed by individual and small hosts.
- Superhost share is consistently high across the top suburbs, with Hunters Hill leading.

<br>

## 📊 Dashboard

The Power BI report, **Sydney Airbnb Analysis**, is built on `listings_clean.csv` and includes:

| Visual | Purpose |
|:---|:---|
| KPI cards | Total listings, unique hosts, median price and Superhost percentage |
| Listings by Suburb | Shows supply concentration |
| Median Price by Suburb | Compares pricing across the top suburbs |
| Total Listings by Host Type | Shows the Commercial, Individual and Small split |
| Superhost Percentage by Suburb | Compares host quality signals |
| Room type slicer | Filters every visual by Entire home or apartment and Private room |

The report file is available in the `powerbi` folder.

<br>

## 🧪 Methodology

The full workflow lives in `notebooks/01_airbnb_cleaning.ipynb` and follows fourteen documented steps.

| Step | Task | Purpose |
|:---:|:---|:---|
| 1 | Load raw file | Read IDs as `int64` to avoid precision loss when joining |
| 2 | Audit missing data | Separate fully empty columns from partially missing ones |
| 3 | Profile activity | Measure dormant, long stay and commercial listings |
| 4 | Visual exploration | Inspect price, reviews, minimum nights, host size and suburbs |
| 5 | Remove unneeded columns | Drop empty, redundant, URL, scrape metadata and personal fields |
| 6 | Convert price | Strip currency symbols and cast to numeric |
| 7 | Repair bathrooms | Parse `bathrooms_text` and create a shared bathroom flag |
| 8 | Repair bedrooms | Apply room type rules and guest capacity medians, with a missing flag |
| 9 | Extract amenities | Parse JSON amenity lists into a count and five feature flags |
| 10 | Create features | Build activity, stay type, host type and Superhost fields |
| 11 | Filter listings | Keep active, short stay, priced entire homes and private rooms |
| 12 | Cap extreme prices | Winsorise at the 99th percentile into `price_capped` |
| 13 | Select and validate | Keep final columns and run sanity checks by room type and suburb |
| 14 | Export | Save `listings_clean.csv` for modelling and Power BI |

### Filtering Funnel

Four rules narrow the data to the target market:

1. At least one review in the last 12 months, used as a proxy for activity.
2. Minimum stay under 28 nights, to isolate short stay rentals.
3. A valid nightly price, so every row supports pricing analysis.
4. Entire homes and private rooms only, because hotel rooms (56) and shared rooms (42) have too few records for reliable comparison.

### Data Quality Decisions

- **Personal information removed.** Host names, descriptions and locations are dropped. Results are reported by suburb and host type only.
- **Target leakage check.** Estimated occupancy and revenue were compared against recent review counts. Matching counts suggest the estimates are derived from review activity, so reviews should not be used to predict those estimates.
- **Outlier handling.** Prices above 34,000 AUD per night were capped rather than deleted, preserving the row while protecting downstream models. The original `price` column is retained for reporting.

<br>

## 🗂️ Dataset

| Item | Detail |
|:---|:---|
| Source file | `listings.csv` (Inside Airbnb style Sydney export) |
| Raw size | 20,573 rows |
| Cleaned output | `listings_clean.csv` |
| Granularity | One row per listing |

The raw data file is not committed to this repository. See [How to Run](#-how-to-run) for where to place it.

<br>

## ⚙️ Feature Engineering

| Feature | Description |
|:---|:---|
| `is_active` | 1 if the listing has at least one review in the last 12 months |
| `stay_type` | Short stay (under 28 nights) or Long stay (28 nights or more) |
| `host_type` | Individual (1 listing), Small (2 to 4), Commercial (5 or more) |
| `is_superhost` | Numeric flag derived from host status |
| `is_shared_bath` | 1 if the bathroom is shared |
| `amenity_count` | Total number of amenities listed |
| `has_pool`, `has_parking`, `has_aircon`, `has_wifi`, `has_kitchen` | Binary amenity flags |
| `bedrooms_was_missing` | Audit flag for imputed bedroom values |
| `price_capped` | Price winsorised at the 99th percentile for modelling |

<br>

## 📁 Repository Structure

```
sydney-airbnb-analysis/
├── data/
│   ├── raw/                  # listings.csv (not tracked)
│   └── processed/
│       └── listings_clean.csv
├── notebooks/
│   └── 01_airbnb_cleaning.ipynb
├── powerbi/
│   └── sydney_airbnb_analysis.pbix
├── requirements.txt
├── .gitignore
└── README.md
```

<br>

## 🚀 How to Run

**1. Clone the repository**

```bash
git clone https://github.com/your-username/sydney-airbnb-analysis.git
cd sydney-airbnb-analysis
```

**2. Install dependencies**

```bash
pip install -r requirements.txt
```

**3. Add the raw data**

Place `listings.csv` inside `data/raw/` and update `RAW_PATH` in the first notebook cell if needed.

**4. Run the notebook**

```bash
jupyter notebook notebooks/01_airbnb_cleaning.ipynb
```

Run the cells in order and review the printed output after each step. The cleaned file is written to `listings_clean.csv`.

**5. Open the dashboard**

Open `powerbi/sydney_airbnb_analysis.pbix` in Power BI Desktop and refresh the data source to point to your local `listings_clean.csv`.

<br>

## 🧰 Tech Stack

| Area | Tools |
|:---|:---|
| Language | Python 3 |
| Data processing | pandas, NumPy |
| Visualisation | Matplotlib, Power BI |
| Environment | Jupyter Notebook |
| Version control | Git and GitHub |

<br>

## 🗺️ Roadmap

- [x] Data cleaning and feature engineering
- [x] Power BI dashboard
- [ ] Add review sentiment scores from `reviews.csv`
- [ ] Build a fair value price prediction model
- [ ] Identify under priced and over priced listings by suburb
- [ ] Add a second dashboard page for host and revenue analysis

<br>

## 👤 Author

**JOSHUA PEREIRA**
Data Analyst based in Sydney, Australia



<br>

<div align="center">

If you found this project useful, consider giving the repository a star.

</div>
