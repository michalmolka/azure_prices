# Azure Retail Prices Exporter (`azure_prices`)

A lightweight Python / Jupyter notebook utility for fetching, filtering, and exporting cloud pricing data from the official **Azure Retail Prices REST API** (`https://prices.azure.com/api/retail/prices`) into **Parquet** and **Excel (`.xlsx`)** formats using **Polars**.

---

## What's Inside

- **[`costs_call.ipynb`](./costs_call.ipynb)** — Main notebook containing two workflows:
  1. **Full Catalog Dump & Consolidation**
     - Paginates through the entire Azure Retail Prices API (`NextPageLink`, ~950k records in USD) and stores raw JSON records locally.
     - Consolidates all JSON records into a single **Parquet** file (`output_data/all_USD.parquet`) and a formatted **Excel** workbook (`output_data/all_USD.xlsx`).
     - Extracts unique sorted lists of `armRegionName`, `productName`, and `serviceName` to make discovering filter values easy.
  2. **Targeted OData Filtered Queries**
     - Builds OData `$filter` queries dynamically from a Python dictionary (e.g., filtering by `currencyCode`, `location`, `type`, `serviceName`, `productName`).
     - Fetches matching SKU/meter prices directly from the API, orders key pricing columns first (`retailPrice`, `unitPrice`, `skuName`, `meterName`, `unitOfMeasure`, etc.), and exports the result to an autofitted Excel sheet (`output_data/filtered_USD.xlsx`).

---

## Requirements

```bash
pip install requests polars xlsxwriter
```

---

## Quick Usage

1. Open [`costs_call.ipynb`](./costs_call.ipynb) in Jupyter or VS Code.
2. To fetch only specific service prices (fastest), jump to the **`# GET REST API DATA WITH FILTERS`** section, adjust the `params` dictionary:
   ```python
   params = {
       "currencyCode": ["USD"],
       "location": ["US East"],
       "type": ["Consumption"],
       "serviceName": ["Azure Cognitive Search"],
       "productName": ["Azure AI Search"],
   }
   ```
3. Run the cells to query the API and generate `output_data/filtered_USD.xlsx`.
