# Walmart Hurricane Analytics Project

## Project Overview

This project examines Walmart sales and inventory data from September 2024 to understand how data quality issues affect business analysis, particularly when evaluating product demand and potential hurricane-related purchasing patterns.

The project uses Python, pandas, SQLite, and data visualization to clean transaction records, standardize inconsistent identifiers, consolidate fragmented product SKUs, and evaluate sales performance.

## Repository Structure

The repository contains:

- `data/` — Original datasets used in the project.
- `notebooks/` — Jupyter notebooks containing the data cleaning, validation, and analysis.
- `CHARTER.md` — Team charter outlining project responsibilities.
- `README.md` — Project overview, execution instructions, and reflections.

## Requirements

The project uses Python 3 and Jupyter Notebook, along with the following libraries:

- pandas
- numpy
- matplotlib
- sqlite3 (included with Python)

Additional libraries may be required depending on the notebook.

## How to Run the Code

1. Clone or download this GitHub repository.
2. Open the project in Jupyter Notebook or Google Colab.
3. Install any missing Python libraries.
4. Open the notebook located in the `notebooks/` folder.
5. Ensure the datasets in the `data/` folder are accessible and that the file paths in the notebook point to the correct locations.
6. Run the notebook cells sequentially, beginning with the imports and data-loading steps.
7. Review the cleaning decision log, validation results, and generated visualizations.

The notebook should be run from beginning to end to ensure that all variables and cleaned datasets are created in the correct order.

## Data Cleaning and Validation

The original transaction dataset contained 25,562 records. After removing 30 exact duplicate records, the cleaned dataset contained 25,532 records.

The cleaning process addressed several issues:

- **Store identifiers:** Standardized inconsistent capitalization, spaces, and hyphens, reducing 60 variations to 12 unique store IDs.
- **Product categories:** Standardized 37 category labels into 10 consistent categories.
- **Unit prices:** Converted prices stored as text into numeric values. Validation confirmed no missing, negative, or zero prices.
- **Timestamps:** Converted valid timestamps into a consistent datetime format while retaining and flagging 884 records with missing timestamps.
- **Negative quantities:** Preserved and flagged 130 transactions with negative quantities because they may represent returns or inventory adjustments.
- **Product identifiers:** Preserved original SKUs during cleaning and used product catalog mappings for the SKU consolidation analysis.

All cleaning decisions were documented in a decision log with their justifications, affected records, and implementation status. This decision log is located within its own dropdown.

Validation confirmed that the cleaned dataset contained no remaining exact duplicates, repeated transaction IDs, or missing store identifiers. The difference of 59 units between the original and cleaned quantity totals was fully explained by the 30 removed duplicate records.

## Analysis: SKU Consolidation

The analysis examined four product groups with fragmented SKU identifiers: Strawberry Pop-Tarts, First-Aid Kits, Light Beer, and White Bread.

Consolidating equivalent product identifiers allowed sales quantities and estimated revenue to be evaluated at the product-group level.

Estimated net revenue by product group:

| Product Group        | Estimated Net Revenue |
| -------------------- | --------------------: |
| Light Beer           |            $48,753.07 |
| First-Aid Kits       |            $27,316.89 |
| Strawberry Pop-Tarts |            $13,544.50 |
| White Bread          |             $5,694.62 |
| **Total**            |        **$95,309.08** |

Strawberry Pop-Tarts were examined in greater detail using daily sales data. Across September 2024, the three associated SKUs accounted for 3,884 net packages, including 3,752 packages with valid timestamps.

Consolidated daily sales peaked at 540 packages on September 14, compared with 49 packages on September 1. This demonstrates how analyzing fragmented identifiers separately can obscure the total demand for a product.

The SKU mapping process preserved all 25,532 cleaned transaction records and the original cleaned quantity total of 49,343 units.

## Limitations

The dataset contains 884 transactions with missing timestamps. These records were retained in overall calculations but could not be assigned to specific dates.

Additionally, negative quantities were preserved because their exact business meaning could not be independently verified. Consequently, sales quantities and revenue should be interpreted as net estimates.

The daily sales analysis identifies changes in purchasing activity but does not establish whether those changes were caused by hurricane-related conditions.

The planned weather API integration was not completed, limiting the ability to investigate relationships between weather conditions and sales.

## Reflection Questions

### 1. Data Quality Impact

One significant data quality issue was inconsistent SKU identifiers for equivalent products. For example, Strawberry Pop-Tarts appeared under three different identifiers, which made the product's total sales difficult to evaluate when each SKU was analyzed separately. By using the product catalog to group equivalent SKUs, I was able to calculate combined sales quantities and estimated revenue. This revealed that Strawberry Pop-Tarts reached 540 net packages sold on September 14, a much larger total than any individual SKU showed. Without identifying this issue, the analysis could have understated product demand and overlooked important purchasing patterns.

### 2. ETL Trade-offs

One important decision involved handling missing timestamps. I chose to retain transactions with missing dates and flag them rather than remove them from the cleaned dataset. An alternative would have been to exclude these transactions entirely, which would have simplified the daily sales analysis but reduced the accuracy of overall sales totals. For Strawberry Pop-Tarts alone, 132 net packages could not be assigned to specific dates. Removing those transactions would have understated total product sales. Retaining them allowed the analysis to preserve overall quantities while acknowledging the limitations of the daily timeline.

### 3. Pipeline Trust

I agree that the data pipeline presents a major risk because errors introduced during cleaning or integration can affect every subsequent analysis. In this project, SKU consolidation was particularly sensitive because incorrectly grouping products could distort demand estimates. I addressed this by preserving original identifiers, using catalog mappings, and validating that the mapping process did not change the total number of transactions or the total quantity sold. I also checked for duplicate records, missing values, and inconsistent identifiers. These checks were important because a seemingly minor cleaning error could have produced misleading results that appeared reasonable in the final visualizations.

### 4. Business vs. Data

The board-games analysis required evaluating business risk using imperfect information. I was assigned this question, but the weather API integration that my analysis depended on was not completed by my teammate. As a result, I could not conduct the intended analysis or make a sufficiently supported recommendation. Making a business decision without the relevant weather information could lead to incorrect assumptions about purchasing patterns and inventory needs. With additional time, I would prioritize completing the data integration, validating the merged records, and comparing sales patterns before recommending any inventory changes.

### 5. Team Collaboration

Our team divided responsibilities across the data preparation, integration, and analysis tasks. I was responsible for data cleaning, documenting cleaning decisions, Question 2, and Question 5. I completed the cleaning pipeline, decision log, and SKU consolidation analysis for Question 2. However, my teammate did not complete the assigned weather API integration, which prevented me from completing Question 5 as planned. This experience highlighted the importance of communication and accountability when different parts of a project depend on one another. With another week, I would establish earlier progress checkpoints and clearer deadlines for dependent tasks. My strongest contribution was developing and validating a reproducible cleaning process that preserved the integrity of the transaction data. I cannot accurately assess my teammate's technical contribution because the assigned integration work was not delivered.
