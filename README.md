# API_TO_AZURE
---
```
This Repro Explain the overall the process of to connect and manipulate the Live_API data in Azure Cloud Platform
```
---
# Get the Live_Data
---

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/846d695d-b9b1-452c-bd2a-e9e102b3f058" />

---
# 🎯 First-Step-Process
---

## 🧭 **STEP 1: Create the Azure Data Factory**

### 🔹 1. Click **+ Create** (in the middle of your screen)

You’ll see this blue button under the “No resources match your filters” message.

---

### 🔹 2. In the search bar that appears, type:

```
Data Factory
```

Then choose **“Azure Data Factory”** from the list.

---

### 🔹 3. Click **Create** on the Azure Data Factory page

Now you’ll be taken to the **Create Data Factory** configuration form.

---

### 🔹 4. Fill out the required fields:

* **Subscription** → (keep your current subscription)
* **Resource Group** → select **APITOAZURE_DATA_FACTORY** (the one you’re in)
* **Region** → choose one close to you (for example, *Central India* or *East US*)
* **Name** → give it a unique name, e.g.

  ```
  AlphaAPIDataFactory
  ```
* **Version** → select **V2**

Then click **Next: Git Configuration** (you can skip Git for now) → click **Next: Review + Create** → **Create**.

---

### 🔹 5. Wait for deployment

This will take 1–2 minutes. Once it’s done, click **“Go to resource.”**

---

### 🔹 6. Launch ADF Studio

Inside your new Data Factory resource page, click the blue **“Launch Studio”** button.

That will open the **Azure Data Factory Studio** at:

```
https://adf.azure.com/
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e724ae8d-e76b-46a1-8d83-35ef88edcc2e" />

Perfect 👏 — your screenshot confirms that your **Azure Data Factory deployment is complete!** 🎉

Now you’re ready to move to the next part — actually **opening the Data Factory Studio** so you can connect it to your Alpha Vantage API.

---

## ✅ Next Steps — Open Your Data Factory Studio

### 🔹 Step 1: Click **Go to resource**

You’ll find this blue button on your screen (as shown in the image).
Clicking it will take you to your **Data Factory overview** page.

---

### 🔹 Step 2: Launch ADF Studio

Once you’re on the overview page, look for a big blue button labeled:

```
Launch Studio
```

Click it — this will open a new tab that looks like:

```
https://adf.azure.com/…
```

That is your **Azure Data Factory Studio workspace**, where you can:

* Author pipelines 🧩
* Manage linked services ⚙️
* Monitor data runs 📊

---

### 🔹 Step 3: Verify the Studio opens correctly

You should see a left-hand menu with:

* **Home**
* **Author**
* **Monitor**
* **Manage**

If you see these, everything is set up correctly!

---
# after launch the studio then you 



Excellent 👏 You’ve done it perfectly — by clicking **Launch Studio**, you’ve now opened the **Azure Data Factory Studio**, where you’ll build your actual pipeline.

Let’s now go through **Step 2 — Creating the REST Linked Service** in full detail.
This is where you connect Azure Data Factory to the **Alpha Vantage API** using the API key you got earlier (`E26ZDMWV4LYTFKUB`).

---

## 🧩 STEP 2: Create a REST Linked Service in Azure Data Factory

### 🎯 Purpose

A **Linked Service** is basically a connection configuration.
You’ll use it to tell ADF how to reach Alpha Vantage’s API (the source of your data).

---

### 🔹 Step 2.1: Go to “Manage” in ADF Studio

1. In the **left-hand menu**, click the **⚙️ Manage** tab.
   (It’s the gear icon near the bottom of the left panel.)
2. Then select **Linked services** under the “Connections” section.

---

### 🔹 Step 2.2: Create a new Linked Service

1. Click the **+ New** button at the top.
2. A pop-up will appear asking you to choose a data store type.

---

### 🔹 Step 2.3: Search for “REST”

In the search box, type **REST** → select **REST** → click **Continue**.

---

### 🔹 Step 2.4: Configure the REST Linked Service

Now, fill in the details:

| Field                                    | Value                               |
| ---------------------------------------- | ----------------------------------- |
| **Name**                                 | `AlphaVantage_REST`                 |
| **Description**                          | Connects to Alpha Vantage Stock API |
| **Base URL**                             | `https://www.alphavantage.co/`      |
| **Authentication type**                  | `Anonymous`                         |
| **Enable server certificate validation** | Leave checked                       |
| **Proxy**                                | Leave blank (unless you use one)    |

📌 **Note:** Alpha Vantage uses the API key as a **query parameter**, not as a header — that’s why you use **Anonymous** authentication.

---

### 🔹 Step 2.5: Test the connection

Click **Test connection** at the bottom.

If successful ✅, you’ll see:

> “Connection successful.”

Then click **Create**.

---

### 🎉 Result:

You’ve now created a Linked Service that allows your Azure Data Factory pipelines to connect to **Alpha Vantage API**.

---

### 🪄 What’s Next (Step 3 Preview)

In **Step 3**, we’ll create a **Dataset** that uses this linked service — it defines **what endpoint to call**, for example:

```
query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=E26ZDMWV4LYTFKUB
```

This will let ADF pull the stock data for Microsoft (MSFT).

---

# DataFactoryStudio
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/617ece7a-4ce9-404d-9531-33a3de1946fa" />

# Copy the Rest Data in Data Lake

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ba4ac7dd-3396-45ca-ac9a-a25d617224b0" />


---

## 🧩 **STEP 3: Create REST Dataset (Source – Alpha Vantage API)**

### 🎯 Goal

You will create a dataset that connects to the **Alpha Vantage API** and fetches the stock data (like daily or intraday prices).

---

### 🔹 Step 3.1: Open the Author Tab

1. In your Data Factory Studio, look at the **left panel**.
2. Click on the **✏️ “Author”** icon (the pencil).
3. Under the **“Factory Resources”** section, expand **Datasets**.
4. Click **+ New dataset**.

---

### 🔹 Step 3.2: Choose REST as Dataset Type

1. A new window appears — it asks what type of dataset you want.
2. In the search bar, type **“REST”**.
3. Select **REST** → click **Continue**.

---

### 🔹 Step 3.3: Link to Your REST Linked Service

Now, ADF asks: “Which REST connection do you want to use?”

1. Choose the **Linked Service** you already made (`AlphaVantage_REST`).

   * If you forgot the name, it’s the one created in Step 2.
2. Under **Request method**, select **GET** (since we are retrieving data).

---

### 🔹 Step 3.4: Enter the API URL Details

You will now tell ADF *which* Alpha Vantage endpoint to call.

In the **Relative URL** box, paste this example 👇
(You can replace `MSFT` with any stock symbol you want.)

```
query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=YOUR_API_KEY
```

🧠 **Explanation:**

* `function=TIME_SERIES_DAILY` → fetches daily stock prices.
* `symbol=MSFT` → tells API which company (e.g., Microsoft).
* `apikey=YOUR_API_KEY` → replace with your own Alpha Vantage key.

🔹 Example with real key:

```
query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=E26ZDMWV4LYTFKUB
```

---

### 🔹 Step 3.5: Define Response Format

ADF needs to know what data type it’s getting from the API.

| Field            | Value                     |
| ---------------- | ------------------------- |
| Request method   | GET                       |
| Format           | JSON                      |
| Relative URL     | (Your API endpoint above) |
| Request body     | Leave blank               |
| Pagination rules | Leave blank for now       |

---

### 🔹 Step 3.6: Preview the API Response

1. Click **Preview Data** (bottom toolbar).
2. You should see a JSON object — something like:

   ```json
   {
     "Meta Data": {...},
     "Time Series (Daily)": {
       "2025-11-03": { "1. open": "426.50", "2. high": "430.00", ... },
       ...
     }
   }
   ```
3. ✅ If you see data — that means your REST dataset is working!

---

### 🔹 Step 3.7: Save and Name the Dataset

* Dataset name:

  ```
  AlphaVantage_Daily_MSFT
  ```
* Description:

  ```
  Daily stock prices for Microsoft from Alpha Vantage API.
  ```
* Click **OK → Publish All** (top toolbar).

---

### ✅ **Result**

You now have a **REST dataset** connected to the **Alpha Vantage API**.
ADF can now **pull live JSON data** every time the pipeline runs.

---

# Sink DataSet

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb1e70ae-beee-49a6-9d33-c09b0620bd83" />


---

## 🧩 STEP 4 — Create the Sink Dataset (Azure Data Lake JSON)

### 🎯 Goal:

We want to **store** the API output from Alpha Vantage into your **Azure Data Lake Storage (ADLS Gen2)** in **JSON format**.

---

### 🔹 4.1 — Open Author Tab and Create New Dataset

1. In the left menu, click the **✏️ Author** icon (it looks like a pencil).
2. Under **Factory Resources**, find **Datasets**.
3. Click the **“+” icon** next to *Datasets* → select **New dataset**.

---

### 🔹 4.2 — Choose Data Store Type

You’ll now see a list of data source options.

1. In the search box, type **“Data Lake”**.
2. Select **Azure Data Lake Storage Gen2**.
3. Then choose the **format** → select **JSON** (since Alpha Vantage returns JSON).
4. Click **Continue**.

✅ Now you’re creating a dataset that will write to your Data Lake as a JSON file.

---

### 🔹 4.3 — Create or Select Linked Service

You need to link this dataset to your actual Data Lake storage account.

1. Under **Linked service**, click **+ New**.
2. A new window opens → fill in the details:

   * **Name:** `ADLS_LinkedService`
   * **Account selection method:** *From Azure subscription*
   * **Azure subscription:** select your subscription
   * **Storage account name:** choose your existing Azure Data Lake account
   * **Authentication type:** *Account key* (you can use Managed Identity if you’ve set it up)
3. Click **Test connection** → should say ✅ *Connection successful*
4. Then click **Create**

🧠 This tells ADF how to reach your Data Lake.

---

### 🔹 4.4 — Configure File Path & Format

Now back in the dataset screen, you’ll set *where* the file will go.

* **Linked service:** `ADLS_LinkedService`

* **File path:**

  * Example:

    ```
    container/folder/alpha_stockdata.json
    ```

    If your container is `alpha`, it might look like:

    ```
    alpha/output/alpha_stockdata.json
    ```
  * Note: ADF will create the folders automatically if they don’t exist.

* **File format:** JSON

* **Compression type:** None

✅ Tip: Use a clean folder structure like `/alpha/outputs/` so future files are organized.

---

### 🔹 4.5 — Set Dataset Name and Save

* On the top-right panel → change **Name** to:
  `AlphaVantage_Json_Sink`
* Click **OK**
* Then at the top → click **Publish All** to save everything.

---

### 🧠 What You’ve Done So Far:

| Component                 | Purpose                        |
| ------------------------- | ------------------------------ |
| **Linked Service (REST)** | Connects to Alpha Vantage API  |
| **Dataset (REST)**        | Reads the data from API        |
| **Linked Service (ADLS)** | Connects to Azure Data Lake    |
| **Dataset (ADLS JSON)**   | Stores API data into JSON file |

---

###import schema - None

Excellent 🎉 — that means your **Sink Dataset (Data Lake JSON)** has been created successfully!
Now you’re ready for the **final and most important part: Step 5 → Create the Copy Pipeline** — this is where the actual data transfer (from the API to the Data Lake) happens.

Let’s go step-by-step very clearly 👇

---

## 🧩 **STEP 5 — Create the Copy Pipeline**

### 🎯 Goal

You’ll create a **Copy Data pipeline** that:

> Reads data from your REST API (source)
> ➜ Writes it into Azure Data Lake (JSON sink)

---

### 🔹 **5.1 — Create a New Pipeline**

1. In the left sidebar, under **Factory Resources**, click the **“+” button**.
2. Choose **Pipeline → Copy data**.
3. Give it a name like

   ```
   Copy_RestToDataLake
   ```

---

### 🔹 **5.2 — Add Source (REST API)**

1. In the pipeline editor (middle of the screen), click **+ Add Source**.
2. Choose **Dataset → RestResource1** (that’s the REST dataset you created).
3. Verify that the linked service is your `AlphaVantage_REST`.
4. Click **Preview data** (optional) → you should see sample API output (JSON).

✅ *This confirms your API connection works.*

---

### 🔹 **5.3 — Add Sink (Data Lake JSON)**

1. Scroll down and click **+ Add Sink**.
2. Choose **Dataset → AlphaVantage_Json_Sink** (the one you just created).
3. Confirm the linked service points to your Azure Data Lake.
4. You can leave:

   * File name option: *Auto-generate* (ADF will create a new file each run)
   * Or specify a fixed name like `alpha_stockdata.json`

---

### 🔹 **5.4 — (💡 Optional) Map Data**

If both datasets have schemas, you’d map fields here —
but since you chose **Import schema = None**, just skip this section.
ADF will copy the **entire JSON response** as-is.

---

### 🔹 **5.5 — Validate and Publish**

1. Click **Validate All** (top bar).

   * Make sure you see ✅ “No errors found.”
2. Click **Publish All** to save your pipeline.

---

### 🔹 **5.6 — Debug or Trigger a Run**

Now you’re ready to test it!

1. Click **Debug** → ADF will run the pipeline immediately.
2. Watch the progress at the bottom pane → you should see:
   ✅ *Copy activity completed successfully.*
3. Once done, go to your **Azure Data Lake Storage → container → folder** you set earlier.

   * You’ll find a new file like:

     ```
     alpha/output/alpha_stockdata.json
     ```
   * Download it — you’ll see the same JSON from your REST API saved there.

---

### 🧠 What You’ve Achieved

| Component           | Role                          |
| ------------------- | ----------------------------- |
| REST Linked Service | Connects to Alpha Vantage API |
| REST Dataset        | Reads JSON data               |
| ADLS Linked Service | Connects to Azure Data Lake   |
| JSON Dataset (Sink) | Defines output location       |
| Copy Pipeline       | Moves data → API → Data Lake  |

---

# Publish All
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6cb35129-f675-47fb-bb99-c5c99a8399f4" />

No problem — I’ll walk you through **every single click and text box** from the screen you’re on, step-by-step, until the pipeline runs and you can see the file in your Data Lake. Follow these exact steps in Azure Data Factory Studio.

---

## Quick context (where you are)

You’re on the **Dataset (JSON sink)** screen for your ADLS dataset (file path `alpha / output / alpha_stockdata.json`). We’ll finish saving that dataset, create a pipeline, run it, and verify the file in ADLS.

---

# PART A — Finish & publish the sink dataset

1. **Select the Linked service**

   * On the screen (Connection tab) find **Linked service** dropdown → make sure it shows your ADLS linked service (e.g. `ADLS_LinkedService`).
   * If not present: click **+ New** → create linked service using your storage account (follow the prompt, Test connection → Create).

2. **Test the connection**

   * Click **Test connection** (small link under Linked service).
   * If you see **Connection successful**, continue. If not, fix credentials.

3. **Confirm File path**

   * In the three File path boxes type:

     * 1st box (container): `alpha`
     * 2nd box (folder): `output`
     * 3rd box (file): `alpha_stockdata.json`
   * (Optional) Click the folder icon at the right to browse containers if you prefer.

4. **Compression & Encoding**

   * Compression type: **No compression**
   * Encoding: **Default(UTF-8)**

5. **Rename the dataset (clearer)**

   * On the right Properties pane → **Name**: replace with `AlphaVantage_Json_Sink`
   * (Optional) Description: `Sink for AlphaVantage daily time series JSON.`

6. **Save dataset**

   * Click **OK** (bottom-right). The dataset is saved to your authoring canvas but **not yet published**.

7. **Publish changes**

   * Top toolbar: click **Publish all** (blue button).
   * Wait until the publish completes (notification appears). Now the dataset is committed to the service.

---

# PART B — Create the copy pipeline (move data REST → Data Lake)

1. **Create new pipeline**

   * Left panel: click the **+** (plus) next to **Factory Resources** or use the big **+** (New) and choose **Pipeline → Pipeline**.
   * In the canvas, click the pipeline name and rename it `Copy_RestToDataLake` (top left of canvas).

2. **Add Copy Data activity**

   * In the **Activities** toolbox (left side), expand **Move & transform** and drag **Copy data** onto the center canvas.

3. **Configure Source (REST dataset)**

   * Click the **Copy Data** activity to open its settings (tabs: General, Source, Sink, Mapping, Settings).
   * Click **Source** tab.
   * **Source dataset**: click the dataset picker and choose your REST dataset (e.g., `RestResource1` or `AlphaVantage_Daily_MSFT`).
   * If your REST dataset has **Relative URL** already set (query?function=...&apikey=...), you can test preview:

     * Click **Preview data** → it should show JSON. If it fails, edit the REST dataset relative URL to include your API key and symbol.

4. **Configure Sink (ADLS dataset)**

   * Click the **Sink** tab of the Copy activity.
   * **Sink dataset**: choose `AlphaVantage_Json_Sink` (the dataset you created).
   * By default ADF may overwrite the file. To **avoid overwriting** and create a unique file each run, set a dynamic file name:

     * Under **Sink** find **File path type** (or look for **Sink > Settings > File name option**). Choose **Output to single file** or **Enter filename as expression** depending on the UI.
     * Click the small **Add dynamic content** (fx) next to the file name box and paste this expression:

       ```
       @concat('alpha_stockdata_', formatDateTime(utcNow(), 'yyyyMMdd_HHmmss'), '.json')
       ```
     * This will create files like `alpha_stockdata_20251103_203400.json` for each run.
   * If the UI doesn’t show an expression editor, leave the dataset filename empty (ADF may auto-generate) or put a fixed name if you want overwrite.

5. **Skip Mapping**

   * Click **Mapping** tab — since you used `Import schema = None`, no mapping required. Leave default.

6. **Validate pipeline**

   * Top toolbar of pipeline editor: click **Validate all**. Ensure you see **No errors found**.

7. **Publish your pipeline**

   * Click **Publish all** (top bar). Wait until publish completes.

---

# PART C — Run (Debug) the pipeline and check results

1. **Run now (Debug)**

   * In pipeline canvas, click **Add trigger → Debug** (or click the **Debug** button).
   * The pipeline will start. A bottom pane shows run progress.

2. **Monitor run**

   * If the Copy activity status shows **Succeeded**, great. If it fails, click the activity in the run output to see error details (common errors: REST endpoint wrong, API key missing, storage permission issues).

3. **Verify the file in Data Lake**

   * Go to **Azure Portal** → open your **Storage account** (the ADLS Gen2 you linked).
   * Under **Storage Explorer (preview)** or **Containers**, open container `alpha` → folder `output`.
   * You should see a file named like `alpha_stockdata_YYYYMMDD_HHMMSS.json` (or `alpha_stockdata.json` if fixed).
   * Click the file → **Download** or **Open** → you’ll see the raw JSON returned by the API.

---

# PART D — Troubleshooting tips (if something fails)

* **REST Preview fails**

  * Open your REST dataset → ensure **Relative URL** includes `&apikey=E26ZDMWV4LYTFKUB` and correct `symbol=MSFT` (or other).
  * Try copy the full endpoint into a browser to confirm it returns JSON:

    ```
    https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=E26ZDMWV4LYTFKUB
    ```

* **Storage write fails**

  * Re-run **Test connection** on ADLS linked service.
  * Ensure the storage account key or managed identity has **write** permissions to the container.

* **File not appearing**

  * Check pipeline run history in the **Monitor** tab (left menu) → open run details → check sink copy logs for target path.

---

If you want, I can give you the **exact clicks** to set the dynamic filename depending on which UI elements you see (some Azure UI versions show different small menus). Tell me whether you want a fixed filename (overwrite) or unique filenames per run — I’ll give the exact sequence for that setting.


# Step 5 — Create the Copy Pipeline so your REST API data actually gets copied into your Data Lake.
---
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/59cc7082-9674-4483-9ad8-60272c15366f" />



Perfect 👏 You’ve successfully finished **Step 4 (Sink Dataset)** — that “Publishing completed” green message confirms everything’s saved correctly.
Now let’s move to **Step 5 — Create the Copy Pipeline** so your REST API data actually gets copied into your Data Lake.

Follow these exact steps from your current screen:

---

### 🧩 **STEP 5 — Create the Copy Pipeline**

#### **1️⃣ Open a new pipeline**

* On the **left panel**, click the **“+” (plus)** icon next to **Factory Resources**.
* Select **Pipeline → Pipeline**.
* A new blank pipeline canvas opens.
* On the right, in **Properties > Name**, rename it to something clear:

  ```
  Copy_REST_to_ADLS
  ```

---

#### **2️⃣ Add a Copy Data activity**

* On the **left toolbar**, open **Move & transform**.
* Drag the **Copy data** activity onto the middle canvas.

---

#### **3️⃣ Configure the Source**

* Click the Copy Data activity box once.
* In the bottom pane, go to the **Source** tab.
* Click **+ New** (or use dropdown) and select your **REST dataset** → `RestResource1`.
* This tells Data Factory to *read* data from your API.
* (Optional) Click **Preview data** — if everything is correct, you’ll see a JSON sample of your Alpha Vantage output.

---

#### **4️⃣ Configure the Sink**

* Go to the **Sink** tab (next to Source).
* Choose the dataset you just made for the Data Lake, e.g. `ADLS_LinkedService`.
* This is where ADF will *write* the JSON file.
* **(Optional)**: if you want a **unique filename each run**, click the **fx** (Add Dynamic Content) button beside the file name field and paste:

  ```adf
  @concat('alpha_stockdata_', formatDateTime(utcNow(), 'yyyyMMdd_HHmmss'), '.json')
  ```

  → this avoids overwriting existing files.

---

#### **5️⃣ Skip Mapping**

* Click the **Mapping** tab → leave blank (ADF automatically maps JSON fields).

---

#### **6️⃣ Validate your pipeline**

* Top bar → click **Validate all** ✅
* Ensure you see “No validation errors found”.

---

#### **7️⃣ Publish your pipeline**

* Top bar → click **Publish all** again.
* Wait for “Publishing completed”.

---

#### **8️⃣ Run the pipeline**

* Click **Add trigger → Debug** (or **Trigger now** if published).
* ADF will execute the Copy activity.

---

#### **9️⃣ Check results**

* When it finishes, you’ll see **Status: Succeeded** in the bottom output window.
* Then go to **Monitor (tab on left)** → verify the run logs.
* Finally, open your **Storage Account → Containers → alpha/output/** folder — your JSON file should be there 🎉.

---

Would you like me to show how to set the *dynamic filename* (so it doesn’t overwrite each time) or keep a *fixed filename* (`alpha_stockdata.json`)?

# View the output
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/546b985d-229f-4c88-a72d-2ac48bbd2042" />

Excellent 👍 you’re in the **right place** — that’s your **Azure Storage Account** (`apistockdata001`).

Now let’s find the **container** (this is where your API data file is stored). Follow these **exact steps** 👇

---

### 🧩 STEP 1 — Open the Storage Account

1. Click on your storage account name — **`apistockdata001`**.
   (It’s blue and clickable in the list.)

---

### 🧩 STEP 2 — Go to “Containers”

2. In the left-hand menu (under **Data storage**), click **“Containers”**.
   This is where Azure Blob Storage organizes your folders.

---

### 🧩 STEP 3 — Open Your Container

3. You’ll see one or more containers — for example:

   * `alpha`
   * `output`
   * or the name you gave in the **ADF Sink dataset** (e.g. `stockdata`).

4. Click on that container name.
   Inside it, you’ll see your **copied file** from ADF — something like:

   ```
   alphavantage_data.json
   ```

   or

   ```
   output/alphavantage_data_20251103.json
   ```

---

### 🧩 STEP 4 — View Your File

5. Click the file → then click **“View/edit”** or **“Open”**.
   You should now see the **JSON content** (your API data).

---

Would you like me to show (with screenshots or steps) how to **connect this JSON file from your Data Lake to Power BI** next?

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eea0bf70-d70c-41f7-9cfd-e017505b21a9" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3fb78b3-1714-4978-9fc6-8975c881b3c3" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/463f8ace-b11e-4c15-9518-8c8322efdf8c" />




