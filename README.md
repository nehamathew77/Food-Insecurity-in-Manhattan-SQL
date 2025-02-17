Food Swamps in NYC: Exploring Food Deserts, Income, and Access

****
Overview

This project investigates the prevalence of food swamps in New York City and their correlation with neighborhood income levels. Inspired by my previous work in nutrition education and firsthand experience with food insecurity in underfunded areas, this analysis uses public datasets to explore how the density of fast food restaurants compares to grocery store availability—and how these factors relate to the income levels of various zip codes in NYC.

****

Part 1: Topic and Dataset Selection

Topic Selection
Chosen Topic:
Analyzing food swamps in NYC by correlating fast food restaurant density with income levels across different zip codes.
Why This Topic?
I am passionate about nutrition and public health—having worked for a college nutrition department in underfunded communities—and I want to understand how similar issues affect NYC. Food swamps (areas dominated by unhealthy food options) have been linked to adverse health outcomes, and this project will help illuminate whether lower-income zip codes are more susceptible.
Dataset Selection
Datasets Used:
NYC Restaurant Data:
A dataset containing records for nearly 250,000 restaurants with 26 columns of data (including names, locations, and safety ratings). For this project, the data is filtered to include only the top 15 fast food restaurant chains based on location (zip codes).
Income Data:
A custom table containing zip codes, the median household income for each area, and the percentage of high-income households (income > $200k).
Grocery Store Data:
A dataset detailing the number of supermarkets available in each zip code.
Data Characteristics:
Format: Structured, tabular data sourced from the NYC Health database (a reliable, public data source).
Preparation: All missing values and duplicate records were removed during initial data cleaning in Python, and the dataset was filtered to focus on the most relevant records.

****

Part 2: Dataset Cleaning and Restoration

Data Import & Cleaning:
The CSV files were imported into PGAdmin for deeper SQL analysis. A new table was created for each dataset with matching column headers and appropriate data types.
Cleaning Process:
Removed missing values and duplicate rows.
Filtered the restaurant dataset to focus on the top 15 fast food chains.
Ensured consistency across datasets to facilitate JOIN operations later.
Integration:
Datasets from different sources were integrated using JOINs on zip codes, allowing us to merge fast food counts, income levels, and grocery store counts for comprehensive analysis.

****

Part 3: Query Development and Testing

Central Question
What zip codes in NYC could be classified as food deserts/swamps, and is there a correlation with the income levels of those zip codes?

Query Scenarios
Scenario 1: Fast Food Restaurant Density by Zip Code
Objective: Determine the number of fast food restaurants in each zip code.
SQL Query:
SELECT "ZIPCODE", COUNT("DBA") AS fast_food_count
FROM public."Restaurant_Data"
GROUP BY "ZIPCODE"
ORDER BY fast_food_count DESC;
Insight: Initial results showed a high concentration in Midtown, suggesting factors like commuter traffic might influence fast food density.

Scenario 2: Wealthiest and Poorest Zip Codes
Objective: Identify the top 5 highest and lowest median household income zip codes.
SQL Queries:
SELECT "Zip_Code", "Median_Household_Income"
FROM public."Income_By_Zipcode_NYC"
ORDER BY "Median_Household_Income" DESC
LIMIT 5;
SELECT "Zip_Code", "Median_Household_Income"
FROM public."Income_By_Zipcode_NYC"
ORDER BY "Median_Household_Income" ASC
LIMIT 5;
Insight: Comparing income data with fast food density helps gauge whether lower-income areas experience higher concentrations of fast food options.

Scenario 3: Merging Fast Food Density with Income Data
Objective: Explore the relationship between fast food concentration and income by joining restaurant and income data.
SQL Query:
SELECT 
    r."ZIPCODE", 
    COUNT(r."DBA") AS fast_food_count, 
    i."Median_Household_Income", 
    i."High_Income_Households"
FROM 
    public."Restaurant_Data" r
JOIN 
    public."Income_By_Zipcode_NYC" i 
ON 
    r."ZIPCODE" = i."Zip_Code"
GROUP BY 
    r."ZIPCODE", i."Median_Household_Income", i."High_Income_Households"
ORDER BY 
    fast_food_count DESC;
Insight: This query aims to uncover patterns between fast food density and income levels, revealing unexpected high concentrations in business-centric areas like Midtown.

Scenario 4: Grocery Store Distribution and Income
Objective: Assess whether areas with fewer supermarkets also have lower median incomes or fewer high-income households.
SQL Query:
SELECT 
    i."Zip_Code", 
    i."Median_Household_Income", 
    COUNT(g."DBA Name") AS grocery_store_count
FROM 
    public."Income_By_Zipcode_NYC" i
LEFT JOIN 
    public."Grocery_Stores" g 
ON 
    i."Zip_Code" = g."Zip Code"
GROUP BY 
    i."Zip_Code", i."Median_Household_Income"
ORDER BY 
    grocery_store_count DESC;
Insight: This analysis helps to evaluate the accessibility of healthier food options relative to income levels.

Scenario 5: Fast Food to Grocery Store Ratio
Objective: Compare the concentration of fast food restaurants to grocery stores, factoring in the percentage of high-income households.
SQL Query:
SELECT 
    g."Zip Code", 
    COUNT(DISTINCT r."CAMIS") AS fast_food_count,
    COUNT(DISTINCT g."License Number") AS grocery_store_count,
    (COUNT(DISTINCT r."CAMIS")::decimal / NULLIF(COUNT(DISTINCT g."License Number"), 0)) AS fast_food_to_grocery_ratio,
    i."High_Income_Households" AS high_income_percentage
FROM 
    public."Grocery_Stores" g
LEFT JOIN 
    public."Restaurant_Data" r
ON 
    g."Zip Code" = r."ZIPCODE"
LEFT JOIN 
    public."Income_By_Zipcode_NYC" i
ON 
    g."Zip Code" = i."Zip_Code"
GROUP BY 
    g."Zip Code", i."High_Income_Households"
ORDER BY 
    high_income_percentage ASC;
Insight: This final query provides a ratio to determine how balanced—or imbalanced—the local food environment is, and whether it correlates with income demographics.

****

Part 4: Final Presentation

Presentation Format:
A 5-15 minute recording summarizing the project's key insights and the SQL querying process.
Slide Highlights:
Introduction & Motivation: Personal background, project rationale, and the central question.
Data Overview: Source details, dataset structure, and cleaning process.
Key Insights: Visuals from Tableau dashboards highlighting fast food density, income correlations, and supermarket distribution.
Reflections: Discussion on the SQL techniques used (especially JOINs) and potential next steps in data analysis (e.g., regression analysis).

****
Part 5: Course Reflection and Professional Outlook

Learning Milestone:
Mastering JOINS in SQL was transformative, enabling me to combine multiple datasets and extract meaningful insights from complex data.
Career Alignment:
The SQL and data analysis skills acquired are directly applicable to roles in data analytics, public health, and even political data analysis—areas where data-driven decision making is crucial.
Future Applications:
I plan to enhance my analysis with more complex queries, integrate additional datasets (like borough-level food insecurity measures), and utilize advanced visualization tools (Tableau) and machine learning techniques.
Community Impact:
I aim to share these findings with community organizations to help target food insecurity in NYC, ensuring that resources are directed where they’re most needed.
Tools & Technologies

Python: Data cleaning and preprocessing.
SQL & PGAdmin: Data querying and analysis.
Tableau: Data visualization and dashboard creation.
How to Run the Project

Data Cleaning & Preparation:
Run the provided Python scripts to clean and filter the datasets.
Data Import:
Use PGAdmin to import the CSV files, ensuring the table structures match the datasets.
Query Execution:
Execute the SQL queries provided in this README within PGAdmin to reproduce the analysis.
Visualization:
Open the Tableau workbook to interact with the visualizations that supplement the SQL findings.

Data Sources:
NYC Health database for restaurant data, public income datasets, and grocery store records.
Course Instructors:
Special thanks to the SQL and data analysis instructors who provided invaluable guidance throughout this project.
