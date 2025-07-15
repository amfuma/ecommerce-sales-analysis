# 📊 E-Commerce Data Exploration Notes

**Dataset:** Online Retail – Sample of 10,000 Rows  
**Source:** UCI Machine Learning Repository  
**Initial Load Date:** 07/14/2025  
**Analyst:** Anthony Fumagalli

---

## 🔍 Step 1: Dataset Structure

| Column Name | Description                                      |
|-------------|------------------------------------------------|
| InvoiceNo   | Transaction ID – starts with “C” for cancelled orders |
| StockCode   | Product ID                                     |
| Description | Product name/description                       |
| Quantity    | Number of items purchased (can be negative for returns) |
| InvoiceDate | Date and time of transaction                   |
| UnitPrice   | Price per unit                                 |
| CustomerID  | Unique customer identifier (some missing)     |
| Country     | Country where transaction took place          |

---

## 🧼 Step 2: Initial Data Observations

- **CustomerID:** 2291 rows have missing CustomerID  
  Likely represent anonymous, guest, or B2B orders without client tracking  

- **Quantity:** 130 rows have negative quantities  
  Most likely indicate returns or order corrections  

- **InvoiceDate:** Date range in sample: 2010-12-01 to 2010-12-05  
  Includes timestamp (may trim later if not needed)  

- **Country Distribution:** 15 unique countries represented  
  Dominated by:  
  - UK: 9402 rows  
  - Germany: 181  
  - EIRE (Ireland): 109  
  - France: 106  

---

## 🧹 Step 3: Data Cleaning

- **CustomerID (Missing Values):** Removed 2291 rows with blank CustomerID, as they cannot be tracked for customer-level behavior.  

- **Returned Orders:** Created a new column called Returned? using:  
  `=IF(D2<0, "Yes", "No")`  
  This flags negative Quantity values as returns or cancellations.  
  Returns were not deleted, but kept for future return-rate analysis.  

- **Invoice Date Clean-Up:** Renamed InvoiceDate to InvoiceDT to reflect timestamp.  
  Created new column IDate using: `=TO_DATE(F2)`  
  Formatted IDate to show only the date (e.g., 12/1/2010).  

- **Total Price Column:** Created new column TotalPrice using:  
  `=IF(D2>=0, D2 * H2, "")`  
  This returns total price for sold items, and blank for returned products to keep revenue clean.  

---

## 📊 Step 4: Exploratory Business Analysis

### 🔥 Top 10 Products by Revenue
- Filtered out returned transactions from TotalPrice column.  
- Used UNIQUE() on Description and SUMIF() to calculate total revenue per product.  
- Sorted and retained only the top 10 revenue-generating products.  
- Result: Clean two-column summary of Product Name and Revenue Generated.

### 💰 Top 10 Customers by Purchase Behavior
- Created UNIQUE() list of CustomerIDs.  
- Calculated total amount spent using SUMIFS() and total items purchased using COUNTIF().  
- Sorted by total revenue and listed top 10 customers.

### 🔄 Return Rate Analysis
- Filtered for Returned? = Yes, yielding 100 rows.  
- After removing rows with missing CustomerID, total remaining rows = 7,708.  
- Return rate = 100 / 7,708 = 1.3%.  
- Noted potential for deeper breakdown by product, country, or customer in Tableau.
