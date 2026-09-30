# Mall Customer Segmentation

**Team:** _add names_, _add names_, _add names_
**Brief:** DS-03 (Intermediate)  ·  **Dataset:** Mall Customer Segmentation Data (Kaggle), 200 customers × 5 columns

## The question
A shopping mall's marketing team sends the same promotion to every member. **What natural customer groups exist based on income, spending and age, and what marketing action suits each group?** (Unsupervised: no target variable; success metric = silhouette score + one actionable campaign per group.)

## Key results
- **Five clear customer groups** found with K-Means on income and spending score (k = 5 chosen by both the elbow method and the best silhouette score).
- K-Means silhouette **0.555** vs **0.332** for a simple median-split rule (baseline).
- Income alone does not predict spending (correlation 0.01), and **all 62 high spenders (score > 60) are aged 40 or under**.
- Biggest opportunity: **Careful High Earners**, 35 customers with ~88 k$ income but a spending score of only ~17.
- Five personas with one campaign each: Premium Spenders (VIP retention), Careful High Earners (personal shopper / premium trials), Young Trend Seekers (social-media flash deals), Budget-Conscious Shoppers (coupons), Average Regulars (loyalty points).

![Five customer segments with centroids](reports/figures/10_clusters.png)
*Five customer segments with centroids*

![All high spenders are 40 or younger](reports/figures/06_age_vs_spending.png)
*All high spenders are 40 or younger*

## Approach
1. **Cleaning:** renamed the misspelled `Genre` column to `Gender` and removed spaces/brackets from column names; no missing values or duplicates; 2 income outliers (137 k$) kept as real high earners.
2. **EDA:** 7 charts (gender, age, income, spending, income vs spending, age vs spending, correlation), each with a written insight.
3. **Model:** features scaled with StandardScaler; baseline = median-split quadrants; K-Means with `random_state=42`, k = 5; evaluated with silhouette, per-customer silhouette (error analysis), stability across seeds, and a 3-feature (Age) comparison.

## How to run
```bash
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```
Open `notebooks/analysis.ipynb` and choose **Run All** (Kernel → Restart & Run All). The notebook reads `data/mall_customers.csv` and saves the charts to `reports/figures/`.

**Google Colab:** open the notebook in Colab and choose Runtime → Run all; a **Choose Files** button appears, so upload `data/mall_customers.csv`.

## Repository structure
```
ds-03-mall-customer-segmentation/
├── README.md
├── data/
│   └── mall_customers.csv
├── notebooks/
│   └── analysis.ipynb      # full notebook, outputs visible
├── reports/
│   ├── figures/            # key charts saved with plt.savefig()
│   └── presentation.pdf    # demo-day slides
└── requirements.txt
```

## Limitations
- Only 200 customers and 4 usable columns.
- The spending score is a mall-made index whose formula is unknown.
- Clusters describe *who* customers are, not *why* they spend; campaigns must be tested (A/B) before full roll-out.
- 9 customers sit on cluster borders, so their persona is uncertain.

## AI usage
Claude (Anthropic) was used as an assistant to draft notebook code, chart code and documentation text, and to suggest checks (for example data-quality and leakage checks). Every team member re-ran the notebook, checked each result against the outputs, and can explain every cell and decision in their own words. The team is responsible for the final content.
