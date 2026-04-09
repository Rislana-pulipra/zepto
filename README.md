*Zepto SQL Data Analysis Project*
#  Zepto SQL Data Analysis Project

##  Project Overview

This project focuses on analyzing product-level data from a Zepto-like grocery delivery platform using SQL. The goal is to extract meaningful business insights related to pricing, discounts, stock availability, and category performance.

The dataset contains product information such as category, price, discount, quantity, and stock status.

---

##  Dataset Description

The dataset is stored in a table named `zepto` with the following columns:

| Column Name              | Description                        |
| ------------------------ | ---------------------------------- |
| serial_id                | Unique identifier for each product |
| category                 | Product category                   |
| name                     | Product name                       |
| mrp                      | Maximum Retail Price               |
| discount_percent         | Discount applied (%)               |
| available_quantity       | Quantity available                 |
| discounted_selling_price | Final selling price                |
| weight_in_gms            | Product weight                     |
| out_of_stock             | Stock availability (TRUE/FALSE)    |
| quantity                 | Quantity per unit                  |

---

##  Database Setup

```sql
CREATE TABLE ZEPTO (
    serial_id INT PRIMARY KEY,
    Category VARCHAR(150),
    name VARCHAR(150),
    mrp INT,
    discount_Percent FLOAT,
    available_Quantity FLOAT,
    discounted_Selling_Price FLOAT,
    weight_In_Gms FLOAT,
    out_Of_Stock BOOL,
    quantity INT
);
```

---

##  Data Cleaning Steps

* Removed rows where `MRP = 0`
* Converted price from **paise to rupees**
* Checked for NULL values across all columns

```sql
DELETE FROM zepto WHERE mrp = 0;

UPDATE zepto
SET mrp = mrp / 100.0,
    discounted_selling_price = discounted_selling_price / 100.0;

--conver paise into rupees
update zepto
set mrp=mrp/100.0,
discounted_selling_price = discounted_selling_price/100.0 ;

select mrp,discounted_selling_price from zepto;

```


---

##  Key Business Questions & Insights

### 🔹 1. Top 10 Best Value Products

* Identified products with highest discount percentages

```sql
SELECT name, mrp, discount_percent
FROM zepto
ORDER BY discount_percent DESC
LIMIT 10;
```

---

### 🔹 2. High-Value Out-of-Stock Products

* Found expensive products currently unavailable

```sql
SELECT name, mrp
FROM zepto
WHERE out_of_stock = TRUE AND mrp > 300
ORDER BY mrp DESC;
```

---

### 🔹 3. Revenue Estimation by Category

* Calculated potential revenue per category

```sql
SELECT category,
SUM(discounted_selling_price * available_quantity) AS total_revenue
FROM zepto
GROUP BY category;
```

---

### 🔹 4. Expensive Products with Low Discounts

* Products with MRP > ₹500 but discount < 10%

```sql
SELECT name, mrp, discount_percent
FROM zepto
WHERE mrp > 500 AND discount_percent < 10;
```

---

### 🔹 5. Top Categories by Average Discount

* Identified categories offering best deals

```sql
SELECT category,
AVG(discount_percent) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

---

### 🔹 6. Price per Gram Analysis

* Best value products based on weight

```sql
SELECT name, weight_in_gms, discounted_selling_price,
ROUND((discounted_selling_price / weight_in_gms)::numeric, 2) AS price_per_gms
FROM zepto
WHERE weight_in_gms >= 100
ORDER BY price_per_gms;
```

---

### 🔹 7. Product Weight Categorization

* Grouped into Low, Medium, Bulk

```sql
SELECT name, weight_in_gms,
CASE
    WHEN weight_in_gms < 1000 THEN 'Low'
    WHEN weight_in_gms < 5000 THEN 'Medium'
    ELSE 'Bulk'
END AS weight_category
FROM zepto;
```

---

### 🔹 8. Total Inventory Weight by Category

* Helps in logistics and warehouse planning

```sql
SELECT category,
SUM(weight_in_gms * available_quantity) AS total_weight
FROM zepto
GROUP BY category;
```

---

##  Key Findings

* Some products offer **very high discounts**, making them attractive for customers.
* Certain **high-MRP products are out of stock**, indicating demand-supply gaps.
* Categories vary significantly in **revenue contribution**.
* **Low-discount expensive products** may need pricing strategy improvements.
* Weight-based pricing helps identify **best value products**.
* Inventory weight insights help in **logistics optimization**.

---

##  Tools Used

* PostgreSQL / pgAdmin
* SQL (Data Cleaning & Analysis)

---




