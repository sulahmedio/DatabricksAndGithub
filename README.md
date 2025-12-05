# EPA CO₂ Emissions Lakehouse Pipeline

## 📌 Overview
This project demonstrates an end-to-end **data engineering workflow** using **Databricks, SQL, Delta Lake, and Power BI**.  
I ingested public EPA CO₂ emissions data from U.S. counties, applied data validation logic, transformed raw fields into analytics-ready tables, and built visualizations to identify high-emission regions and population correlations.

---

## 🗂️ Dataset Source
- **Agency:** Environmental Protection Agency (EPA)
- **Year:** 2023
- **Public Access:** [2023 EPA Dataset (ZIP)](https://www.epa.gov/system/files/other-files/2024-10/2023_data_summary_spreadsheets.zip)
- **Dashboard (Databricks):** [United States Emissions Breakdown Dashboard](https://dbc-fe7fe398-1f4d.cloud.databricks.com/dashboardsv3/01f0d0cd53dd1b9c94c262ffd8fc4b0d/published?o=188684523093722&f_6226f2a4%7E3df1a6a8=%257B%2522columns%2522%253A%255B%2522y%2522%255D%252C%2522rows%2522%253A%255B%255B%2522Maricopa%2520County%252C%2520AZ%2522%255D%255D%257D&f_6226f2a4%7E9e4f25fb=%257B%2522columns%2522%253A%255B%2522coordinates_latitude%2522%252C%2522coordinates_longitude%2522%255D%252C%2522rows%2522%253A%255B%255B%252243.878831%2522%252C%2522-107.669052%2522%255D%255D%257D)

## The raw CSV data includes fields such as:
- County & State
- Latitude / Longitude
- Total Emissions
- Emissions Per Person
- Population

---

## 🧰 Technology Stack
- **Databricks Notebooks**
- **SQL (Databricks Workspace)**
- **PySpark (basic transformation logic)**
- **Delta Lake (Bronze → Silver tables)**
- **Power BI (visual analytics)**

---

## 🔍 Data Quality & Validation Rules
Applied rules before transformation:
1. **Null checks:** County, State, and Total Emissions must not be null
2. **Domain validation:** State abbreviation must be one of the 50 U.S. states
3. **Range enforcement:** `emissions_per_person > 0`
4. **Duplicate removal:** County + State must be unique
5. **Outlier flag:** Highlight values where emissions per person > 5.0 (top 1%)

---

## 🏗️ Architecture

```text
Raw CSV (EPA Data)
        ↓
Databricks Notebook (SQL + Python)
        ↓
Data Quality Validation (null checks, duplicates, domain checks)
        ↓
Delta Table Storage (Bronze → Silver)
        ↓
Power BI Dashboard (analytics layer)
```

🧪 Transformation Examples
Sample SQL (cleaning + type consistency)
```text
SELECT 
    county,
    state,
    CAST(total_emissions AS DOUBLE) AS total_emissions,
    CAST(population AS DOUBLE) AS population,
    ROUND(total_emissions / population, 4) AS emissions_per_person
FROM emissions_raw
WHERE total_emissions > 0
AND population > 0;
```
Delta Table Write
```text
CREATE OR REPLACE TABLE emissions_cleaned
USING DELTA
AS SELECT * FROM emissions_transformed;
```

## 📊 Visualization
The dashboard highlights:
Scatter plot: Emissions per person vs. population
Bar chart: Top 10 highest emitting counties
Donut chart: State-level emissions distribution
Geospatial heatmap: Density of emissions nationwide
Key insight: Top 10 counties represent ~51% of all reported emissions, indicating high concentration in urban and industrial regions.

<img width="2332" height="1071" alt="image" src="https://github.com/user-attachments/assets/ff808aad-0d3d-4073-8198-9af21dd79bfb" />

## 🚀 Key Results
Ingested and cleaned 100k+ EPA records
Built validation, transformation, and analytics layers
Enabled geospatial analysis using embedded lat/long fields
Identified clear emission-per-capita outliers

## 🧠 Lessons Learned
- What an ETL Process is
- Data quality enforcement significantly improves downstream analytics
- Delta storage optimizes queries and supports iterative development
- Structuring data for visualization forced intentional schema and naming decisions

## 📍 Future Enhancements
- Add PySpark deduplication logic automatically
- Implement CDC (change data capture) with Delta Lake versioning
- Add CI/CD pipeline via GitHub Actions for notebook deployment
- Integrate population source data into transformation workflow

---

👤 Author
- Sulaiman Ahmed
- Databricks | Data Engineering & Analytics
- GitHub: https://github.com/sulahmedio
