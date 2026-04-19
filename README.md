# 📊 Inventory Optimization Analysis

🏢 _Company: NovaMart Supply Chain Ltd._

---

## 📌 Project Overview

This project analyzes inventory risk for a building and hardware supply company using MySQL and Metabase. I evaluated 3,000 products worth ₦187.4M, identified 429 low-stock items, and found that 59.7% of products were overstocked. The goal was to highlight stockout risk, excess capital lock-up, and replenishment priorities.

---

## 🔍 Business Questions

- Which products are currently below their reorder level?  
- Which categories are under the most demand pressure?  
- Are there overstocked products tying down capital?  
- Which suppliers contribute the most to inventory?  
- How is inventory distributed across warehouse locations?  
- Which high-value products are at risk of stockout?  

---

## 🛠 Tools & Technologies

- **MySQL** → Data extraction, cleaning, and analysis  
- **Metabase** → Dashboarding and visualization  

---

## 📁 Dataset Description

The dataset contains inventory records for **3,000 products** in a building and hardware supply company.

### Key Fields:
- Product Name  
- Category  
- Supplier  
- Warehouse Location  
- Current Stock  
- Reorder Level  
- Unit Price  

---

## 📊 Key Metrics

- **Total Products:** 3,000  
- **Total Current Stock:** 752,421 units  
- **Total Stock Value:** ₦187,418,229  
- **Average Reorder Level:** 68 units  

---

## 📈 Summary of Key Insights

- 14.3% of products (429 items) are below reorder level → active stockout risk  
- 59.7% of products are overstocked → excess capital tied down  
- Several products are completely out of stock with severe shortages  
- High-value items are close to stockout → direct revenue exposure  
- Hardware and Electrical categories show the highest demand pressure  
- Inventory is evenly distributed across warehouses → storage is not the issue  

---

## 🚨 Critical Findings

### 🔴 High-Urgency Shortages

Products with the largest stock gaps:

- Water Heater (-150)  
- Switch Box (-150)  
- Door Frame (-149)  
- Paint Bucket (-149)  
- Concrete Block (-148)  

👉 These represent complete stockouts and immediate operational failure.

---

### 💰 High-Value Replenishment Risks

Products below reorder level with high financial impact:

- Bulb Pack (₦65,156.52)  
- Toilet Seat (₦60,131.50)  
- Shower Set (₦59,546.04)  
- PVC Pipe (₦58,388.34)  

👉 Delay in restocking here directly affects revenue.

---

### 📦 Category-Level Pressure

- Hardware: 96 low-stock items  
- Electrical: 95 low-stock items  
- Building Materials: 86  
- Interior: 79  
- Plumbing: 77  

👉 Demand is uneven across categories.

---

### 🤝 Supplier Distribution

Top suppliers:

- MegaBuild Ltd: 627 products  
- Jumia Supplies: 611 products  
- FixRight Depot: 609 products  
- Konga Wholesale: 582 products  
- Alpha Materials: 562 products  

👉 Supplier contribution is balanced, but performance tracking is needed.

---

### 📍 Warehouse Distribution

- Warehouse A: 252,361 units  
- Warehouse B: 249,274 units  
- Warehouse C: 250,786 units  

👉 Inventory is evenly distributed across locations.

---

## 💡 Strategic Recommendations

- Shift from fixed reorder levels to demand-driven replenishment  
- Prioritize restocking using both stock levels and product value  
- Reduce excess inventory in slow-moving products  
- Monitor high-pressure categories more frequently  
- Track supplier performance to detect delays early  

---

## 🧾 Sample SQL Queries

```sql
-- Products below reorder level
SELECT
  ProductName,
  (CurrentStock - ReorderLevel) AS Shortage,
  (CurrentStock*UnitPrice) AS Stock_value
  
FROM
  inventory_data
WHERE CurrentStock<ReorderLevel
[[ AND {{Category}} ]]
[[ AND {{Supplier}} ]]
ORDER BY
	CASE {{sort_by}}
		WHEN "Shortage" THEN Shortage 
	END ASC,
	CASE {{sort_by}}
		WHEN "Stock Value" THEN Stock_value
  	END DESC
LIMIT
  {{top_n}}

-- Total Stock Value
SELECT
  CONCAT ("₦", FORMAT (SUM(CurrentStock * UnitPrice),0))
FROM
  inventory_data
WHERE 1=1
[[ AND {{Category}} ]]
[[ AND {{Supplier}} ]]

--Low Stock Count by Category
SELECT Category, COUNT (DISTINCT ProductID) AS TotalProducts
FROM inventory_data
WHERE CurrentStock <= ReorderLevel
[[ AND {{Supplier}} ]]
GROUP BY Category
ORDER BY TotalProducts DESC;
```
---
## 📊 Dashboard

The interactive dashboard (built in Metabase) provides insights into:

- Stock vs Reorder Levels
- Category Performance
- Supplier Contribution
- Warehouse Distribution

<img width="1341" height="646" alt="Dashboard" src="https://github.com/user-attachments/assets/2c8e3107-9c2e-4bd0-8bda-94c615126330" />

## 📃 Business Impact
- Stockout prevention
- Better replenishment prioritization
- Reduced working capital tied up in excess stock
- Category-level visibility for inventory planning

## 📎 Dataset
[inventory_data.csv](https://github.com/user-attachments/files/26839504/inventory_data.csv)
