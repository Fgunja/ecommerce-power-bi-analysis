# E-commerce Sales Analysis (Power BI)

An end-to-end analysis of an e-commerce business. I cleaned messy raw data in Power Query, built a star-schema data model, wrote DAX measures and created a 3-page Power BI dashboard that answers practical business questions.

## Why this project

I built this project to practise the full workflow of a data analyst: getting data, asking business questions, cleaning, modelling, analysing and presenting results. Most datasets on Kaggle are a single, already cleaned table, which skips two important parts of the job (checking relationships between tables and cleaning). So I used an AI-generated dataset based on the structure of a real Kaggle dataset, with deliberately introduced real-world problems: missing values, duplicates, text instead of numbers, inconsistent spelling, impossible values and keys that do not match.

## Business questions

- Where are our customers from?
- What product do we profit from the most?
- Which sales channel is used the most?
- How does the payment method distribution look like?
- How do we increase profit?
- Are there products we should remove?
- At which time of the year do we have the most revenue?

## What I did

### 1. Data cleaning (Power Query)

The dataset has 5 tables: Orders, Order_Items, Customers, Products and Suppliers.

- Trimmed text in every column and standardised capitalisation.
- Removed duplicates by key (Line_ID, Order_ID, Customer_ID, Product_ID).
- Converted text to numbers and dates (removed currency symbols and units, handled decimal separators).
- Standardised inconsistent values (for example `Canceled` to `Cancelled`, `COD` to `Cash On Delivery`, city and country names, gender values).
- Fixed full names written as `SURNAME, Name` and removed extra spaces.
- Replaced `N/A` and impossible values (negative prices, impossible ages and ratings) with `null`.
- Removed order items whose product does not exist in the Products table (merge queries).
- Created an `Age_Group` column.
- Used a `FilePath` parameter instead of a hard-coded file path.

### 2. Data model

Star schema with Orders and Order_Items as fact tables and Customers, Products, Suppliers and a Calendar table as dimensions. All relationships are many-to-one with single-direction filtering. While building the model I found that Products contained duplicate Product_IDs, which caused a many-to-many relationship, and fixed it in Power Query.

### 3. DAX measures

- **Revenue:** quantity x unit price x (1 - discount) for completed orders (`SUMX` inside `CALCULATE`).
- **Profit:** revenue minus quantity x product cost for completed orders (uses `RELATED`).
- **Margin%:** profit divided by revenue (`DIVIDE`).
- **RevenueLY:** revenue in the same period last year (`SAMEPERIODLASTYEAR`).
- **revenueYOY:** year-over-year revenue change.
- **Lost_revenue:** revenue of orders that were not completed.
- **CancelledAndReturned:** number of cancelled and returned orders.
- **TotalOrders:** distinct count of orders.
- **avgorder:** average revenue per order (`AVERAGEX`).

### 4. Dashboard

- **Main dashboard:** overall state of the business with revenue, profit, lost revenue, total orders, average order value, revenue per category, top 5 most profitable products, revenue by country on a map, revenue over time and profit by supplier.
- **Country drill down:** year-over-year revenue with a year slicer, payment method and sales channel distribution, and revenue by age group.
- **Supplier drill down:** supplier country, lead time, best and worst performing products by revenue, and revenue over time for a selected product.

## Key findings

- The most profitable product is Vertex Laptop Max 500.
- The website is the most used sales channel across all countries.
- Credit card is the most used payment method across all countries.
- Revenue peaks from November to January. The best single day was 30.12.2024 with about 17.5k revenue.
- Some products make very little revenue compared with top products from the same supplier. For example, Vertex Skincare Max 800 makes about 1.6k and Terra Fitness Max 600 about 4.3k, while Zenith Laptop Max 400 makes about 266k. These are candidates for removal or reduced focus.

## Recommendations

- Plan marketing campaigns, sales and promo codes for November to January, when revenue is highest.
- Focus campaigns on the countries that already bring the most revenue.
- Promote the most profitable products and consider removing or reducing the weakest ones.

## Limitations

- The data is generated, not real.
- Some orders belong to customers that do not exist in the Customers table, so they appear without customer details in country and age charts.
- Some products have no known production cost, so profit and margin are slightly overstated for them.

## What I learned

This project helped me understand business logic and which visualisations answer the most important questions, such as whether the business is gaining or losing money. I learned how to think about which insights management could use, decide which columns are needed, clean data in Power Query, write DAX measures and build a dashboard that is clear and not oversaturated.

## Tools

Power BI Desktop, Power Query, DAX, Excel.
