# **Event-Driven Data Pipeline using Databricks**

## **Overview**
This project implements an **event-driven data pipeline** using **Databricks**, designed to efficiently ingest, stage, and update data within a **Unity Catalog**-backed architecture. The pipeline is triggered automatically upon the arrival of new files, ensuring **real-time data processing** and **incremental updates**.

## **Architecture**
- **Google Cloud Storage (GCS)** is used as both the **Unity Catalog storage location** and an **external volume** for managing data.
- The pipeline follows a **staging-to-target** model, ensuring that only **new or updated records** are committed to the target table.
- A **Databricks Workflow** monitors the **catalog folder**, and upon detecting a new file, it triggers the processing notebooks automatically.

## **Pipeline Workflow**
1. **Databricks Workflow (Trigger: File Arrival)**
   - Monitors a **catalog folder** for new file arrivals.
   - Automatically triggers the execution of both processing notebooks.

2. **Notebook 1: Data Ingestion & Staging**
   - Reads incoming data from a source.
   - Creates a **staging table** in **Unity Catalog**.
   - Moves the staged data to the **staging path** in Unity Catalog.

3. **Notebook 2: Incremental Update to Target Table**
   - Compares the **staging table** with the **existing target table**.
   - If a matching record exists, updates it with the new incoming details.
   - If no match is found, inserts the record as a new entry.
   - Ensures **data consistency** and avoids duplication.

## **Technologies Used**
- **Databricks** (Notebooks, Delta Tables, Unity Catalog, Workflows)
- **Google Cloud Storage (GCS)** (Used as Unity Catalog location & external volume)
- **Databricks Workflows** (For event-driven execution based on file arrival)
- **Delta Lake** (For handling incremental updates and versioning)
- **PySpark** (For data processing and transformations)

## **Setup & Usage**
1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/databricks-event-driven-pipeline.git
   cd databricks-event-driven-pipeline
   ```
2. **Upload the Notebooks to Databricks**
   - Navigate to **Databricks Workspace** → Import the `.dbc` or `.ipynb` files.

3. **Configure Unity Catalog Storage**
   - Ensure your Databricks workspace has access to **Google Cloud Storage (GCS)**.
   - Set up **Unity Catalog external volumes** pointing to the appropriate GCS bucket.

4. **Configure Databricks Workflow for File Arrival**
   - Create a **Databricks Workflow**.
   - Set the **Trigger** to **File Arrival** on the designated **catalog folder**.
   - Configure it to sequentially execute **Notebook 1 (Staging)** followed by **Notebook 2 (Incremental Update)**.

5. **Test the Pipeline**
   - Upload a new file to the **catalog folder** in Unity Catalog.
   - Monitor the **Databricks Workflow** to verify it detects the file and executes the notebooks.
   - Validate that the **target table** is updated with the latest records.

## **Future Enhancements**
- Integrate **event-driven notifications** (e.g., Google Pub/Sub) to alert users on pipeline execution.
- Implement **schema evolution** handling for dynamic data changes.
---
