# Edmonton Residential Waste Cart Rollout Analysis

A portfolio data-analysis project using **City of Edmonton Open Data** and **2021 Census neighbourhood data**.

## Business Question

How did Edmonton's residential waste-cart rollout vary over time and across neighbourhoods, and how does the picture change after accounting for neighbourhood size, occupied dwellings, and recent population growth?

## Why This Project

A simple ranking of neighbourhoods by cart count is misleading because larger neighbourhoods naturally have more carts.

This project progressively improves the analysis by moving from:

1. **absolute cart counts**
2. to **carts per 1,000 residents**
3. to **carts per 1,000 occupied dwellings**
4. and finally to **rollout intensity vs neighbourhood growth**

The goal is to demonstrate how an analyst can turn an open municipal dataset into a more decision-useful operational view.

## 30-Second Summary

- **2021 dominates the installation-date distribution**, with 389,532 carts represented in the snapshot.
- **The rollout was highly concentrated in time:** about 99% of carts with 2021 install dates fall between January and August.
- **Neighbourhood size changes the story.** Absolute cart totals are less informative than normalized measures.
- **Occupied dwellings are the most useful denominator** in this case because residential carts are more directly linked to households than to population.
- **Fast growth explains some extreme ratios, but not all.** High rollout intensity also appears in established neighbourhoods.
- **Outliers should be investigated, not discarded.** Census timing, rapid development, product mix and data quality can all affect the ratios.

## Portfolio Visuals

### 1. 2021 dominates the observed installation-date distribution

![Active carts by installation year](images/01_installation_year_distribution.png)

The snapshot is strongly concentrated in 2021, which is why the deeper analysis focuses on that year.

### 2. The main 2021 rollout window was January–August

![2021 monthly rollout](images/02_2021_monthly_rollout.png)

Activity is high through August and then drops sharply. November does not appear in the observed installation-date records.

### 3. Dwelling-normalized intensity reveals clear outliers

![Top rollout intensity](images/03_top_rollout_intensity.png)

Most of the upper group is around roughly 1,800–2,400 carts per 1,000 occupied dwellings, while Glenridding Ravine and The Uplands are substantially higher.

### 4. Growth explains only part of the pattern

![Population growth vs rollout intensity](images/04_growth_vs_rollout_selected.png)

This chart shows a **selected set of high-intensity neighbourhoods**, not the full city. Rapidly growing neighbourhoods such as The Uplands and Desrochers Area stand out, but mature areas with near-zero population growth can also have high rollout intensity.

## Data Sources

### City of Edmonton — Cart Counts
Socrata dataset ID: `q2sn-xztr`

Main fields used:

- neighbourhood number
- neighbourhood name
- install date
- product description
- number of carts
- neighbourhood geometry

Observed installation-date range in the working snapshot:

- **Earliest:** 2019-03-10
- **Latest:** 2024-09-10
- **Rows:** 37,734

### City of Edmonton — 2019 Municipal Census
Used as an exploratory historical population baseline by neighbourhood.

### 2021 Federal Census — Edmonton neighbourhood data
Used for:

- actual 2021 neighbourhood population
- occupied private dwellings

## Tools

- Python
- pandas
- NumPy
- requests
- Socrata / REST API
- Matplotlib
- GeoPandas
- Shapely
- Jupyter

## Analysis Workflow

### 1. Data-quality audit

The project first checks:

- observed date range
- product categories
- annual distribution
- missing months
- geometry availability
- differences between metadata descriptions and observed values

This avoids building conclusions on unverified assumptions.

### 2. Rollout timeline

The active-cart snapshot is heavily concentrated in 2021.

Observed carts by installation year:

| Year | Carts represented |
|---:|---:|
| 2019 | 15,760 |
| 2020 | 104,266 |
| 2021 | 389,532 |
| 2022 | 13,194 |
| 2023 | 15,904 |
| 2024 | 7,190 |

Approximately **71%** of carts in the observed installation-date distribution have a 2021 install date.

Within 2021, the rollout was strongly concentrated in the first part of the year:

- January–May: **297,557 carts**
- January–August: **386,125 carts**

That means roughly **99% of the carts with 2021 install dates fall between January and August** in the observed snapshot.

### 3. Neighbourhood rollout

The project identifies:

- top neighbourhoods by month
- each neighbourhood's peak rollout month
- geographic differences in rollout timing

The monthly leaders change substantially across Edmonton, which suggests that rollout timing was not spatially uniform.

### 4. Spatial analysis

Neighbourhood polygons from the Cart Counts dataset are converted into a GeoDataFrame.

Maps include:

- peak rollout month by neighbourhood
- cart rollout intensity by neighbourhood
- neighbourhood growth / rollout segment

### 5. Population normalization

Absolute cart counts are divided by actual 2021 Census population.

This helps reduce the effect of neighbourhood size, but population is not the most operationally relevant denominator for residential carts.

### 6. Occupied-dwelling normalization

The preferred metric in the project is:

**2021-install-date carts per 1,000 occupied dwellings**

This is more directly related to residential cart infrastructure than carts per resident.

Examples of high observed intensity include:

| Neighbourhood | Carts / 1,000 occupied dwellings |
|---|---:|
| Glenridding Ravine | 4,197 |
| The Uplands | 3,695 |
| Hays Ridge Area | 2,410 |
| Desrochers Area | 2,272 |
| Paisley | 2,205 |

These values should **not** be interpreted as literal service-coverage rates.

### 7. Neighbourhood growth

The analysis compares 2019 municipal-census population with 2021 federal-census population as an exploratory growth proxy.

Several rapidly growing neighbourhoods also show high rollout intensity, for example:

- The Uplands
- Desrochers Area
- The Orchards at Ellerslie
- Graydon Hill
- Cy Becker

However, established neighbourhoods such as Mayliewan, McLeod, Capilano and Twin Brooks can also show relatively high rollout intensity.

This suggests that neighbourhood growth explains **part** of the variation, but not all of it.

### 8. Segmentation

Neighbourhoods are divided into four descriptive groups:

- High growth / High rollout
- High growth / Lower rollout
- Lower growth / High rollout
- Lower growth / Lower rollout

This is a descriptive segmentation rather than a performance ranking.

### 9. Outlier detection

The project uses the IQR method to identify unusually high cart-to-dwelling ratios.

Outliers are treated as investigation targets rather than automatic errors.

Possible explanations include:

- rapid residential development after Census Day
- cart product mix
- timing mismatch between Census data and installations
- boundary effects
- data-quality issues

## Main Analytical Takeaways

### 1. 2021 dominates the installation-date distribution
The dataset strongly reflects the 2021 rollout period.

### 2. Rollout timing was concentrated
Most observed 2021 install-date carts fall between January and August.

### 3. Geography matters
Different neighbourhoods reached peak rollout activity in different months.

### 4. Denominator choice materially changes the analysis
Absolute counts, per-capita values and per-dwelling values tell different stories.

### 5. Occupied dwellings are the more useful operational denominator
Residential cart infrastructure is more directly related to dwellings than to total population.

### 6. Growth matters, but it is not the full explanation
Fast-growing neighbourhoods can have very high cart intensity, but high values also appear in established areas.

## Limitations

- Cart Counts is an **active-cart snapshot**, not a complete installation/removal transaction history.
- Census reference dates and cart installation dates are not perfectly aligned.
- Multiple cart types can exist for one dwelling.
- 2019 and 2021 population counts come from different census programs.
- Neighbourhood definitions may change over time.
- Correlation does not establish causation.
- Cart counts do not measure:
  - waste generation
  - collection cost
  - missed collections
  - route efficiency
  - service quality

## Recommended Next Phase

The strongest extension would connect cart infrastructure to more frequently updated operational or development data, such as:

- development permits
- dwelling completions
- 311 waste-service requests
- missed collections
- collection routes
- waste tonnage

A particularly useful next question would be:

> **Can new residential development predict incremental cart demand after the 2021 city-wide rollout?**

## Repository Structure

```text
edmonton_waste_cart_project/
│
├── README.md
├── requirements.txt
└── edmonton_waste_cart_rollout_analysis.ipynb
```

## Author Note

This project was developed as a portfolio case study focused on municipal data analytics, operational reasoning, data quality, GIS and public-sector open data.