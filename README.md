# Dil-py-webscraper
# IBEX DAM Market Data Web Scraping in Databricks

## 🚀 Project Overview

This project demonstrates how to **scrape JavaScript-rendered tables** from the IBEX Day-Ahead Market (DAM) page using **Selenium** and process the data directly inside **Databricks**. The goal was to extract real-time energy pricing and volume data **without relying on APIs or CSVs**, and store the data in **Spark Delta tables**.

🔗 Target URL: [https://ibex.bg/markets/dam/day-ahead-prices-and-volumes-v2-0-2](https://ibex.bg/markets/dam/day-ahead-prices-and-volumes-v2-0-2)

---

## ✅ Requirements

- Must **not use any APIs**
- Must **not manually download CSVs**
- Must **scrape the data dynamically (JavaScript-rendered tables)**
- Must run entirely within **Databricks**
- Output should be structured and stored in **Delta tables**

---

## 🧠 My Approach

1. **Initial Investigation**:
   - Tried using `requests` + `pandas.read_html()` to load the page and extract tables.
   - Realized the tables are **rendered via JavaScript**, so simple HTML scraping returns `null` or incomplete tables.

2. **Explored Alternatives**:
   - Attempted using `requests-html`, `Pyppeteer`, and `AsyncHTMLSession`, but ran into compatibility issues in Databricks' environment.

3. **Final Solution: Selenium in Headless Mode**:
   - Used `Selenium` with **headless Chrome** to render JavaScript dynamically.
   - Parsed HTML with `BeautifulSoup`.
   - Extracted and cleaned relevant data from:
     - Prices and Volumes Table
     - Block Products Table
     - Hourly Products Table

4. **Data Processing in Spark**:
   - Converted the extracted data into **PySpark DataFrames**.
   - Cleaned numeric fields (e.g., removed spaces, replaced commas with dots).
   - Defined custom schemas for each table.
   - Saved the final outputs as Delta Tables:
     - `prices_volumes_table`
     - `block_products_table`
     - `hourly_products_table`

---

## 🛠️ Technologies Used

- **Databricks**
- **Python**
  - `selenium`
  - `beautifulsoup4`
  - `pandas`
- **PySpark**
  - `SparkSession`
  - `DataFrame` with custom schema
- **Delta Lake**
- **ChromeDriver (headless mode)** using `webdriver-manager`

---

## 📂 Output Tables

| Table Name              | Description                                  |
|-------------------------|----------------------------------------------|
| `prices_volumes_table`  | Energy prices and volumes by metric/date     |
| `block_products_table`  | Block-based product prices                   |
| `hourly_products_table` | Hourly market metrics with prices/volumes    |

---

## 📌 How to Run

This notebook is designed to run inside **Databricks**:

1. Paste the code in a Databricks Python notebook.
2. Ensure the required libraries are installed using `%pip install` (e.g., selenium, bs4, pandas, webdriver-manager).
3. Run the cells to scrape, process, and store data as Delta tables.

---

## 🧾 Notes

- The scraper runs in **headless mode** and waits for the page to render before parsing.
- The notebook includes logic to handle:
  - Missing or extra rows
  - Numeric cleaning (e.g., `"1 234,56"` → `1234.56`)
  - Parsing multi-row headers for hourly tables


