
# API_TO_AZURE
> This repository explains the **end-to-end process** of connecting, extracting, and manipulating **Live API Data** inside the **Azure Cloud Platform** using **Azure Data Factory (ADF)** and **Azure Data Lake Storage (ADLS)**.



## Get the Live Data

![Live Data](https://github.com/user-attachments/assets/846d695d-b9b1-452c-bd2a-e9e102b3f058)

---

## Overview of the Process

1. Create and configure **Azure Data Factory**
2. Connect to a **Live REST API (Alpha Vantage)**
3. Create **Linked Services** and **Datasets**
4. Copy the API data into **Azure Data Lake (JSON format)**
5. View and verify data in **Azure Storage Explorer**

---

# STEP 1 — Create the Azure Data Factory

---

1. In Azure Portal → search **“Data Factory”**
2. Click **+ Create**
3. Fill out the required details:

   | Field | Value |
   |--------|--------|
   | **Subscription** | Use your active subscription |
   | **Resource Group** | `APITOAZURE_DATA_FACTORY` |
   | **Region** | Closest region (e.g., *Central India*) |
   | **Name** | `AlphaAPIDataFactory` |
   | **Version** | `V2` |

4. Skip Git configuration → click **Review + Create → Create**
5. Wait for deployment → click **Go to Resource**
6. Click **Launch Studio**

---

![ADF Studio Launch](https://github.com/user-attachments/assets/e724ae8d-e76b-46a1-8d83-35ef88edcc2e)

---

#  STEP 2 — Create a REST Linked Service (API Connection)

---

### 2.1 Navigate to “Manage” Tab

- Click **Manage** on the left panel  
- Select **Linked services → + New**

### 2.2 Select REST

- Search for **REST**
- Choose **REST → Continue**

### 2.3 Configure Connection

| Field | Value |
|-------|--------|
| **Name** | `AlphaVantage_REST` |
| **Base URL** | `https://www.alphavantage.co/` |
| **Authentication type** | `Anonymous` |
| **Enable server certificate validation** |  Checked |

> Alpha Vantage uses API keys as query parameters, so use **Anonymous** authentication.

### 2.4 Test & Create

- Click **Test connection** → should say  *Connection successful*  
- Click **Create**

---

# STEP 3 — Create REST Dataset (Source)

---

### 3.1 Open Author Tab

- Click **Author**
- Expand **Datasets → + New dataset**
- Choose **REST → Continue**

### 3.2 Configure Dataset

- Linked Service → `AlphaVantage_REST`
- Request Method → `GET`
- Format → `JSON`
- Relative URL →  

 ```text
  query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=YOUR_API_KEY
```
Replace with your own symbol and API key.

### 3.3 Preview

Click **Preview Data** → you should see a JSON output similar to:

```json
{
  "Meta Data": {...},
  "Time Series (Daily)": {
    "2025-11-03": { "1. open": "426.50", "2. high": "430.00" }
  }
}
```

### 3.4 Save Dataset

* Name → `AlphaVantage_Daily_MSFT`
* Description → `Daily stock prices for Microsoft from Alpha Vantage API.`
* Click **OK → Publish All**

---

# STEP 4 — Create Sink Dataset (Azure Data Lake - JSON)

---

### 4.1 Create New Dataset

* Go to **Author → + New dataset**
* Choose **Azure Data Lake Storage Gen2**
* Format → **JSON → Continue**

### 4.2 Configure Linked Service

| Field                        | Value                   |
| ---------------------------- | ----------------------- |
| **Name**                     | `ADLS_LinkedService`    |
| **Account selection method** | From Azure Subscription |
| **Storage account name**     | Your ADLS account       |
| **Authentication type**      | Account key             |
| **Test Connection**          | Successful            |

### 4.3 File Path

Example path:

```
alpha/output/alpha_stockdata.json
```

### 4.4 Save Dataset

* Name → `AlphaVantage_Json_Sink`
* Click **OK → Publish All**

**Sink dataset ready!**

---

![Sink Dataset](https://github.com/user-attachments/assets/bb1e70ae-beee-49a6-9d33-c09b0620bd83)

---

# STEP 5 — Create the Copy Pipeline

---

### Goal

Copy live data from **REST API → Azure Data Lake (JSON)**

### 5.1 Create a Pipeline

* Click **+ New → Pipeline**
* Name it: `Copy_RestToDataLake`

### 5.2 Add Copy Activity

* From **Move & Transform**, drag **Copy Data** to the canvas

### 5.3 Configure Source

| Setting            | Value                     |
| ------------------ | ------------------------- |
| **Source Dataset** | `AlphaVantage_Daily_MSFT` |
| **Linked Service** | `AlphaVantage_REST`       |
| **Preview Data**   | Verify JSON output      |

### 5.4 Configure Sink

| Setting              | Value                       |
| -------------------- | --------------------------- |
| **Sink Dataset**     | `AlphaVantage_Json_Sink`    |
| **File Name Option** | (Optional) Use dynamic name |

Dynamic file expression 

```adf
@concat('alpha_stockdata_', formatDateTime(utcNow(), 'yyyyMMdd_HHmmss'), '.json')
```

### 5.5 Validate & Publish

* Click **Validate all** → No errors
* Click **Publish all**

---

# STEP 6 — Run and Monitor Pipeline

---

### 6.1 Trigger the Pipeline

* Click **Add Trigger → Debug**
* Monitor execution in bottom pane

### 6.2 Verify Output

Go to:

> Azure Portal → Storage Account → Containers → `alpha/output`

You’ll find a file like:

```
alpha_stockdata_20251103_203400.json
```

**That’s your live API data saved to Azure!**

---

![Monitor Pipeline](https://github.com/user-attachments/assets/59cc7082-9674-4483-9ad8-60272c15366f)

---

#  STEP 7 — View Data in Azure Storage

---

### Steps:

1. Open **Storage Account → apistockdata001**
2. Go to **Data Storage → Containers**
3. Open **alpha/output/**
4. Click on your JSON file → **View/Edit**

![Data View 1](https://github.com/user-attachments/assets/546b985d-229f-4c88-a72d-2ac48bbd2042)
![Data View 2](https://github.com/user-attachments/assets/eea0bf70-d70c-41f7-9cfd-e017505b21a9)
![Data View 3](https://github.com/user-attachments/assets/c3fb78b3-1714-4978-9fc6-8975c881b3c3)
![Data View 4](https://github.com/user-attachments/assets/463f8ace-b11e-4c15-9518-8c8322efdf8c)

---

# Summary

| Component               | Purpose                    |
| ----------------------- | -------------------------- |
| **REST Linked Service** | Connects to API            |
| **REST Dataset**        | Reads API data             |
| **ADLS Linked Service** | Connects to Azure Storage  |
| **JSON Sink Dataset**   | Stores JSON data           |
| **Pipeline**            | Moves data from API → ADLS |

Note : Every time you must do validate all and Publish all whenever they need.
---
<!--
### Next Steps

* Clean the data using **Data Flow** in ADF
* Transform JSON into tables
* Visualize with **Power BI** or **Synapse Analytics**

---

**Author:** *Azhagammai*
📘 *Project: 

```
Would you like me to also include a **section for Power BI integration (Step 6 — Visualize the data)** at the end of the README?  
I can add it with screenshots and a short guide to connect your Data Lake JSON file to Power BI.
```

-->
