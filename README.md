## 📌 1. Project Overview
This Business Intelligence project, developed in Microsoft Power BI, evaluates e-commerce product performance and optimizes retail pricing strategies. By analyzing the retail_price_project.xlsx dataset, the dashboard uncovers critical relationships between conversion rates, price variances against competitors, and the impact of freight costs on overall sales volume and profitability.

Primary Objective: Optimize pricing and shipping strategies to maximize e-commerce conversion rates and revenue.

Core Metrics: Price Variance, Conversion Rate, Freight Cost Ratio, and Total Revenue.

Tools Used: Power BI (Power Query, DAX, Data Modeling), Microsoft Excel.

# 🎯 2. Business Problems Identified
Price Variance vs. Competitor Benchmarks: Difficulty in tracking how unit prices compare against market competitors and how those price gaps directly impact customer conversion rates.

Freight Cost Friction: Uncertainty regarding how fluctuating or disproportionate freight costs affect total landed product pricing and contribute to cart abandonment.

Conversion Rate Bottlenecks: Lack of visibility into which product categories suffer from low conversion rates despite generating high traffic or impressions.

Profit Margin Erosion: Determining whether heavy discounting or high freight subsidies are eroding net margins for top-selling retail products.

# 🛠️ 3. Data Cleaning & Transformation (Power Query)
The following ETL pipeline was executed in Power Query to prepare the data for analysis:

Data Ingestion & Profiling: Imported retail_price_project.xlsx and audited the dataset for structural integrity and missing values.

Handling Missing Competitor Data: Imputed missing competitor prices (e.g., Comp 1, Comp 2, Comp 3) using category medians to ensure accurate baseline comparisons without skewing from outliers.

Freight & Pricing Standardization: Cleaned null values in freight costs and converted all financial columns (unit_price, freight_price) to fixed decimal/currency types.

Feature Engineering:

Created a Total Price column (Unit Price + Freight Cost).

Developed an Average Competitor Price column to serve as a competitive benchmark.

Date Hierarchy Creation: Standardized date fields to enable seamless Time Intelligence analysis across months and quarters.

# 🔄 4. End-to-End Project Workflow (Steps Done)
Requirement Gathering & KPI Definition: Established core metrics focused on price elasticity, shipping friction, and revenue generation.

ETL & Data Cleaning: Transformed the raw Excel data in Power Query to resolve inconsistencies and engineer new pricing features.

Data Modeling: Built a robust relational data model connecting sales fact tables with product and calendar dimension tables.

DAX Engineering: Authored dynamic measures to calculate conversion rates, average pricing, competitor variance, and freight impact.

Dashboard Design & Visualization: Designed an interactive UI using scatter plots (for price elasticity), matrix visuals (for category performance), and KPI cards to deliver actionable executive insights.

# 📐 5. DAX Measures Library
```// 1. Total Revenue
Total Revenue = 
SUMX(
    'RetailData', 
    'RetailData'[unit_price] * 'RetailData'[qty]
)

// 2. Conversion Rate
Conversion Rate = 
DIVIDE(
    SUM('RetailData'[orders]), 
    SUM('RetailData'[visitors]), 
    0
)

// 3. Average Unit Price
Average Unit Price = 
AVERAGE('RetailData'[unit_price])

// 4. Average Competitor Price
Avg Competitor Price = 
DIVIDE(
    SUM('RetailData'[comp_1]) + SUM('RetailData'[comp_2]) + SUM('RetailData'[comp_3]), 
    3
)

// 5. Price Variance %
Price Variance % = 
DIVIDE(
    [Average Unit Price] - [Avg Competitor Price], 
    [Avg Competitor Price], 
    0
)

// 6. Freight Cost Ratio
Freight Cost Ratio = 
DIVIDE(
    SUM('RetailData'[freight_price]), 
    SUM('RetailData'[unit_price]), 
    0
)
```
# 📊 6. Dashboard Breakdown & Key Insights
Pricing vs. Conversion (Price Elasticity): Products priced 5-10% below the competitor average demonstrated a disproportionately high increase in conversion rates, confirming strong price sensitivity among consumers.

Freight Cost Friction: Freight costs strongly correlate with customer drop-off. Products where the freight cost exceeded 15% of the unit price experienced sharp declines in final conversions, indicating cart abandonment due to shipping shock.

Category Performance: Certain premium product categories maintained stable conversion rates despite higher price variances, highlighting lower price elasticity and stronger brand or product loyalty in those specific segments.

Volume vs. Margin Trade-off: While aggressive discounting drove order volume, it significantly degraded overall profitability for specific fast-moving goods, signaling a need for optimized promotional guardrails.
# 💡 7. Strategic Business Recommendations
Priority Area	Actionable Recommendation	Expected Business Impact
1. Dynamic Competitive Pricing	Implement a dynamic pricing strategy to keep high-elasticity products within a strict 5% variance of the competitor average.	Maximizes conversion rates on price-sensitive items without unnecessary margin sacrifice.
2. Freight Cost Subsidies	Subsidize shipping or offer "Free Shipping" thresholds for high-margin products where freight exceeds 15% of the base price.	Reduces cart abandonment and increases overall sales volume for heavy or bulky items.
3. Premium Category Value	Maintain or slightly increase prices on inelastic premium categories that show resilience to competitive price gaps.	Captures additional margin on products where customers prioritize quality over price.
4. Targeted Promotions	Shift from blanket discounting to targeted promotions, utilizing discounts primarily to clear slow-moving inventory rather than top sellers.	Protects the profitability of fast-moving goods while optimizing overall inventory health.
