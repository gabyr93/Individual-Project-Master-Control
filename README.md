# MasterControl MX Lead Progression — Individual Portfolio

## 1. Business Problem & Objective

MasterControl is a SaaS company offering two core product suites: **QX** (Quality Solutions, 20+ years on the market) and **MX** (Manufacturing Solutions, launched ~4 years ago). MX significantly underperforms QX in converting leads: **12.7% progression rate vs. 19.7%** for QX.

Leadership believes the current targeting strategy sends sales teams after leads that are unlikely to convert — creating missed revenue opportunities and wasting resources. The objective of this project was to identify which industries, company characteristics, and job titles are associated with higher MX lead progression so that Sales and Marketing can prioritize more effectively.

---

## 2. Our Group's Solution

Our team built an interpretable lead scoring framework using MasterControl's internal QAL data, 16,644 records spanning early 2024 through early 2026. The goal was to identify which contact profiles and account characteristics are most likely to convert for the MX product.

We started with EDA to understand data quality and uncover patterns in lead progression across industries, job titles, account tiers, territories, and marketing channels. From there, we engineered features to capture enrichment quality and contact profiles, then compared multiple classification models: logistic regression, random forest, and EBM, using ROC-AUC as the primary evaluation metric. The final model generates probability scores for individual leads, producing a ranked prioritization list that Sales can act on directly. Feature importance analysis was included to keep the model's logic transparent and actionable.

The deliverable is a set of targeting recommendations: the industries and job titles MX sales reps should prioritize, along with suggestions for improving data capture on the MasterControl website.

---

## 3. My Individual Contributions

My work spanned both the EDA and modeling phases. On the EDA side, I handled data setup and cleaning, building a `recipes` based pipeline in R to standardize placeholder values like "Not Enough Info Found" and "Unknown" into a consistent "Low Info" category, so incomplete records could be treated as a signal rather than dropped. From there, I identified a clear pattern: Low Info leads are not randomly distributed. They concentrate in digital/inbound channels, Non-Life Science accounts, smaller account tiers, and EMEA, and they convert at significantly lower rates than well-enriched leads.

On the modeling side, I built the **baseline logistic regression** model, which established the performance benchmark all other models were compared against (ROC-AUC of 0.71), and the **decision tree**, which achieved a ROC-AUC of 0.758 and surfaced the most actionable business insights, showing that priority tier is by far the strongest predictor of MX success, followed by account site function and campaign channel. I also wrote the interpretations for both models.

My individual notebooks are in the `/portfolio` folder of this repo.

---

## 4. Data Source & Reproducibility

This project uses proprietary MasterControl QAL data provided for the case competition. The dataset is **not included in this repository** per competition and data privacy guidelines.

To reproduce the analysis, you would need access to the original `Mastercontrol QAL Performance.csv` file. Notebooks assume it is loaded from a local path. The data dictionary (`MasterControl_Final_QAL_Performance_Data_Dictionary.docx`) describes all variables and their expected missingness levels.

---

## 5. Business Value

The findings from this project directly support MasterControl's goal of increasing MX lead progression from **12.7% toward 16–18%**:

- **Data completeness as a filter**: Leads with incomplete enrichment (site function or manufacturing model missing) progress at under 1% for MX. Deprioritizing these leads, or improving data capture at the point of entry — would immediately improve resource efficiency.
- **Industry targeting**: Pharma & BioTech and Medical Device accounts make up the vast majority of high-enrichment, high-progression MX leads. Non-Life Science accounts are disproportionately associated with low-enrichment records and lower conversion.
- **Channel strategy**: Email and External Demand Gen dominate among well-enriched MX leads, while digital/inbound channels (Online Ads, SEO, Directory Listing) produce a higher share of low-enrichment leads.
- **Website improvements**: The concentration of low-enrichment leads from inbound digital channels suggests that the MasterControl website's data-capture forms may not be collecting sufficient qualifying information from visitors.

In production, these findings translate into a smarter lead scoring model that prioritizes well-enriched accounts in the right industries — allowing Sales to spend time on leads that are statistically more likely to convert.

---

## 6. Difficulties We Encountered

- **Defining "missing"**: True NA values represented only part of the problem. Placeholder strings like "Not Enough Info Found" and "Unknown" are technically present in the data but carry no analytical information. Reconciling these required careful preprocessing and a deliberate decision to treat them as a consistent "Low Info" category rather than imputing or dropping them.
- **Class imbalance**: The MX success rate is only 12.7%, meaning the large majority of leads did not progress. This imbalance required thoughtful evaluation choices beyond simple accuracy.
- **High cardinality in job titles**: The `contact_lead_title` field contained hundreds of raw, free-text entries with inconsistent formatting, making title-based analysis challenging without significant standardization.
- **Leakage-safe feature construction**: Some fields (e.g., `next_stage_c`) could not be used in feature engineering because they reflect the outcome rather than lead attributes known at acquisition time.

---

## 7. What I Learned

This project pushed me to think about data quality differently,  missing values aren't just a problem to fix, they can tell you something meaningful about the data itself. Building the cleaning pipeline and tracing the Low Info pattern to specific channels and industries made that concrete.

On the modeling side, starting with a baseline before jumping to more complex models was a good discipline I'll carry forward. The decision tree also reminded me that interpretability matters, especially when the audience is a business team rather than a technical one.

Beyond the technical work, I learned a lot from working closely with my teammates. Taking feedback, staying accountable to the group's timeline, and building on each other's ideas made the final result stronger than anything I would have done individually.
