# Walmart Hurricane Analytics Project

**University of Virginia | DS2002: Data Science Systems**

## Project Overview

This project examines Walmart transaction, inventory, and product data to understand purchasing patterns surrounding hurricanes in Florida. The analysis focuses on data quality, product identifier inconsistencies, sales trends, and the business implications of inaccurate reporting.

The project uses Python, pandas, SQLite, and data visualization tools to clean and analyze the supplied datasets.

## Repository Structure

```text
walmart-hurricane-project/
├── data/
│   ├── walmart_transactions.csv
│   ├── store_locations.csv
│   ├── product_catalog.csv
│   ├── distribution_centers.csv
│   └── inventory_and_sales.db
├── notebooks/
│   └── [project notebook].ipynb
├── CHARTER.md
└── README.md
```

## Requirements

- Python 3
- Jupyter Notebook or JupyterLab
- pandas
- numpy
- matplotlib
- requests
- sqlite3 (included with Python)

Install the main Python packages using:

```bash
python -m pip install pandas numpy matplotlib requests jupyter
```

Additional packages may be required depending on the imports in the notebook.

## How to Run the Project

1. Download the repository using **Code → Download ZIP** on GitHub, then extract the ZIP file.

2. Open a terminal in the extracted project directory.

3. Install the required Python packages using the command above.

4. Launch Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open the `notebooks` folder and select the project notebook.

6. Verify that all file paths point to the supplied datasets in the `data` folder. Paths may need adjustment depending on the notebook's working directory.

7. Run the notebook cells sequentially from top to bottom using **Kernel → Restart & Run All**.

8. Review the cleaning outputs, decision logs, visualizations, and written analyses.

The notebook must be run with access to the original datasets. Any sections requiring external weather data also depend on successful API access.

## Data Cleaning and Validation

The cleaning pipeline addresses several inconsistencies in the supplied transaction data:

- Removed 30 exact duplicate records.
- Standardized store identifiers from 60 variations into 12 unique store IDs.
- Converted unit prices into numeric values.
- Parsed valid transaction timestamps and flagged 884 missing timestamps.
- Standardized category labels from 37 variations into 10 categories.
- Retained and flagged 130 negative-quantity transactions for review.
- Preserved original SKU identifiers while documenting proposed product mappings.

The cleaning process reduced the transaction dataset from 25,562 to 25,532 records. Validation checks confirmed that duplicate removal accounted for the 59-unit difference between original and cleaned quantity totals.

A decision log documents each cleaning issue, the action taken, its justification, the number of affected records, and its status.

## Question 2: SKU Consolidation

Question 2 investigates how inconsistent product identifiers affect the reported sales volume of Strawberry Pop-Tarts.

Three SKU identifiers were examined:

- `PT-12`
- `PT_STRAWBERRY`
- `SKU#459812`

The analysis combines their quantities into one proposed product group while retaining the original transaction records.

**Key findings:**

- Total Strawberry Pop-Tarts volume: 3,884 packages.
- Packages with valid timestamps: 3,752.
- Packages without valid timestamps: 132.
- Peak daily sales: 540 packages on September 14, 2024.
- Estimated net sales revenue: $13,544.50.

The consolidated daily sales visualization demonstrates how analyzing fragmented SKU identifiers separately can obscure overall demand.

The analysis also compares estimated revenue across four proposed product groups: Strawberry Pop-Tarts, Light Beer, First-Aid Kits, and White Bread.

## SQLite Database

The project includes `inventory_and_sales.db`, a SQLite database containing:

- `daily_sales_summary`
- `inventory_levels`

These tables provide additional inventory and sales information for investigating potential discrepancies between recorded demand and inventory reporting.

## Data Limitations

Several limitations should be considered when interpreting the findings.

Missing timestamps prevent some transactions from being assigned to specific sales dates. Negative quantities were preserved because their interpretation as returns or adjustments could not be independently verified. Proposed SKU consolidations also require product catalog verification before operational implementation.

Daily sales trends and estimated revenue should therefore be interpreted within these data-quality constraints.

## Project Status

The data-cleaning pipeline, cleaning decision log, and Question 2 SKU consolidation analysis have been completed and validated through reconciliation checks.

The remaining analytical sections and weather API integration should be reviewed separately for completeness before treating the entire project as finished.
