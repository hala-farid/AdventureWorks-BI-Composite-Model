# AdventureWorks Enterprise BI & Sales Analytics Suite

An end-to-end Enterprise Business Intelligence and Sales Analytics solution engineered using the **AdventureWorks OLTP** database. This project showcases advanced data modeling, data transformation, and custom reporting capabilities by leveraging a hybrid **Composite Model** (**DirectQuery** for transactional tables and **Import Mode** for dynamic dimensions).

---
# Dashboard Preview

<img width="885" height="500" alt="Sales Report" src="https://github.com/user-attachments/assets/61353040-68d5-4fc5-a76e-01c806909c51" />

## Architecture & Technical Features

### 1. Data Source & Preparation
* **Source:** AdventureWorks OLTP Database ([Download Link & Configuration Guide](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms)).
 
* **Connectivity Mode:** DirectQuery (except for local Power Query transformations).
* **Data Cleansing:** Renamed tables and columns to user-friendly business terms, and meticulously removed unused columns to optimize query performance.
* **Transformations & Merges:** 
  * Consolidated the product hierarchy by merging `Production.Product`, `Production.ProductSubcategory`, and `Production.ProductCategory` into a single, unified, and performance-optimized **Product Dimension** (Contains: `ProductID`, `Product`, `SubCategory`, `Category`) using Power Query M Language.
  * Added order `Status` names based on the database function `ufnGetSalesOrderStatusText`.
  * Generated a dynamic `Dates` table from scratch via Power Query.

### 2. Dimensional Modeling

<img width="1022" height="552" alt="StarSchema" src="https://github.com/user-attachments/assets/e75bc1be-8349-49c8-bbb6-2176bcd4ce92" />

* **Star Schema:** Structured the relationship model around a clean Star Schema, connecting central fact tables to specialized dimension tables.
* **Hierarchies:** Created a custom **Product Hierarchy** (`Category` -> `SubCategory` -> `Product`) and Date hierarchies to allow granular, multi-level slicing.
* **Dynamic Date Relationships:** Handled multiple dates (`OrderDate`, `ShipDate`, and `DueDate`) by establishing inactive relationships with the central date table, dynamically activated using `USERELATIONSHIP` inside DAX measures.

### 3. DAX Tables & Centralized Measures
Created an isolated, dedicated **DAX Measures Table** to host all key business calculations:
* `# Orders` (Count of unique orders)
* `# Qty` (Sum of order quantities)
* `Total SubTotal`
* `Total Tax`
* `Total Freight`
* `Total Due` (Calculated dynamically using DAX to resolve tax, freight, and subtotal metrics)

---

## Dashboard Layout & Visualizations
The dashboard features an intuitive corporate design with a cohesive color palette, structured layout, and highly meaningful visual titles:

* **Executive KPI Cards:** Displaying `# Orders`, `Total SubTotal`, `Total Tax`, `Total Freight`, and `Total Due`.
* **Deep-Dive Analytics:**
  * *# Orders by OrderDate vs. ShipDate vs. DueDate* (Line Chart comparing timeline patterns via active/inactive relationships).
  * *# Orders by Status*
  * *# Orders by Ship Method*
  * *# Orders by Category, SubCategory, and Product* (Using the Product Hierarchy).
  * *# Orders by FlagOnlineOffline* (Online vs. Offline channel split).
  * *# Orders vs. TotalDue by Territory* (Comparing volume and value by geography).
  * *Top 10 Sales Persons* (Dynamic ranking of top performers, customized via the filter pane).
* **Advanced Visual Interactions:**
  * **Drill Down / Drill Up** enabled across hierarchies.
  * **Drill Through** options to deep-dive into detailed granular data pages.

 <img width="1019" height="547" alt="Report DrillThrough" src="https://github.com/user-attachments/assets/ff01b8ef-cf4a-4834-82ca-cd1daa5d72b8" />

 

<img width="998" height="551" alt="Report DrillThrough Page" src="https://github.com/user-attachments/assets/2fc65bf9-8a8c-4b1a-930e-94580d0ca5c7" />

  * Custom-built **Tooltip pages** to reveal instant, context-specific insights on hover.
<img width="984" height="546" alt="ReportToolTip" src="https://github.com/user-attachments/assets/8e23f148-cda6-4acf-8c19-feb21541c103" />


---

## How to Download and Run this Report Locally

Since this report is built using **DirectQuery** connected to a local SQL Server, opening it on your machine for the first time will require pointing it to your local database instance. Follow these steps:

### Prerequisites
1. Install **Power BI Desktop** on your computer.
2. Install **Microsoft SQL Server** and restore the **AdventureWorks** OLTP database using [Microsoft's official guide](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms).

### Steps to Run:
1. **Download the File:** Clone this repository or click on the `.pbix` file above and press **Download**.
2. **Open the Report:** Double-click the downloaded `.pbix` file to open it in Power BI Desktop.
3. **Change the Data Source:**
   * In Power BI Desktop, go to the top ribbon and click **Transform Data** > **Data source settings**.
   * Select the AdventureWorks SQL Server connection from the list and click **Change Source**.
   * Replace the existing server name with your local SQL Server instance name (e.g., `localhost` or `.\SQLEXPRESS`) and make sure the database name points to your restored `AdventureWorks` database.
   * Click **OK**.
4. **Update Credentials:**
   * If prompted, click **Edit Permissions** to configure your database connection credentials (select **Windows** or **Database** credentials depending on your SQL Server security settings).
5. **Refresh and View:**
   * Click **Close** on the settings window and press **Refresh** on the Home tab. 
   * Once updated, the charts and data tables will dynamically query your local SQL Server and display all visuals perfectly!
