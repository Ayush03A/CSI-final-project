# 🚀 Final Project: Enterprise Data Platform - An End-to-End Implementation with Azure 📈

## 📋 Project Overview

This project demonstrates advanced data engineering patterns in **Azure Data Factory (ADF)**, from dynamic API ingestion to high-performance, conditional ELT workflows. The solution emphasizes a secure, automated, and governed approach using Key Vault, Git, and Microsoft Purview.

> 🔗 **Thanks to [CSI (Celebal Summer Internship)](https://www.celebaltech.com/)**   
> This task deepened my understanding of building resilient, cloud-scale data pipelines and solving real-world architectural challenges.

---

## 📋 Project Problem Statement

The goal of this project was to design and build a comprehensive data platform to address several key business and operational requirements for a growing e-commerce company. The specific tasks were:

1.  **Automated API Ingestion:** Create a pipeline to fetch data for 5 specific countries (India, US, UK, China, Russia) from the `restcountries.com` public API. Each country's data must be saved as a separate JSON file, named after the country.

2.  **Time-Specific Automation:** The API ingestion pipeline must be automated to run precisely two times a day: at 12:00 AM and 12:00 PM India Standard Time (IST).

3.  **Conditional Data Processing:** Create a pipeline to handle a daily customer data extract. The core logic should only be processed if the record count for the day is **greater than 500**.

4.  **Complex Nested Logic:** Within the customer pipeline, if the data is being processed (i.e., count > 500), an additional check must be performed. If the customer record count is **greater than 600**, a secondary child pipeline must be triggered to copy related product data.

5.  **Modular & Parameterized Design:** The solution must be designed in a modular way, where the parent customer pipeline can pass the exact record count as a parameter to the child product pipeline.

---

## ✨ Key Features & Architectural Highlights

This project goes beyond the basic requirements to deliver an enterprise-grade data platform. Here are the key architectural decisions that make this solution robust, secure, and production-ready:

| Feature Implemented                | Business Value & Technical Justification                                                                                                                                                                                                                              |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Medallion Architecture**         | 🥉🥈🥇 Data is progressively refined through **Bronze** (raw), **Staging**, and **Silver** (cleansed) layers. This ensures data quality and provides a "single source of truth" in the `Silver.Customers` table, ready for analytics.                  |
| **CI/CD with Git Integration**     | 🔄 All development is version-controlled in Git. A professional `dev` -> `main` workflow with Pull Requests ensures code quality, enables collaboration, and prevents manual errors in production.                                                                       |
| **Secure by Design**               | 🔑 No passwords or sensitive keys are stored in code. **Azure Key Vault** manages all secrets, which are securely accessed at runtime by Azure Data Factory's **Managed Identity**, following zero-trust security principles.                                             |
| **High-Performance ELT**           | ⚡ Instead of slow, traditional ETL, this solution uses a modern ELT (Extract-Load-Transform) pattern. The high-performance **`COPY INTO`** command in Azure Synapse Analytics is used for massively parallel data loading from the data lake.                                                  |
| **Automated Data Governance**      | 🗺️ By integrating with **Microsoft Purview**, the platform benefits from automated data discovery and, most importantly, end-to-end **data lineage tracking**, providing full traceability and building trust in the data.                                    |
| **Resilient & Dynamic Pipelines**  | ⚙️ Pipelines are data-driven, not hardcoded. A "Set Variable" pattern was engineered to handle dynamic filenames reliably, and complex nested logic was solved by creating modular, reusable child pipelines. |

---

## 📂 Project Implementation & Evidence

### 1️⃣ Dynamic API Ingestion & Automation 🌐

**Objective:** Build a fully automated pipeline to fetch country-specific metadata from a public REST API and schedule it to run twice daily in a specific timezone (IST).

#### Implementation Details:
- The pipeline uses a `Lookup` activity to read a list of countries from a central JSON configuration file, making it easily updatable.
- A `ForEach` loop processes all countries concurrently for maximum efficiency.
- A custom **Schedule Trigger** ensures data is refreshed reliably at 12:00 AM and 12:00 PM **India Standard Time**.

📸 **Evidence:**

- ✅ **Pipeline Design:** The `PL_Ingest_Countries_To_Bronze` pipeline showing the `Lookup` -> `ForEach` pattern.

  ![Pipeline for Dynamic API Ingestion]([LINK_TO_SCREENSHOT_04])

- ✅ **Scheduled Trigger:** The trigger configured for twice-daily execution in the India Standard Time timezone.

  ![Twice-Daily IST Trigger]([LINK_TO_SCREENSHOT_05])

- ✅ **Successful Execution:** The pipeline successfully created separate JSON files for each country in the ADLS Bronze layer.

  ![Successful API Ingestion Run and Output Files]([LINK_TO_SCREENSHOT_06])

---

### 2️⃣ Conditional, High-Performance Database ELT 📈

**Objective:** Create a robust pipeline to ingest customer data, but only process it fully if the record count meets specific business rules, including triggering a child pipeline for related data.

#### Implementation Details:
- A modern **ELT (Extract-Load-Transform)** pattern was implemented using the high-performance **`COPY INTO`** command.
- An **`If Condition`** checks if the record count is > 500. More complex logic for the > 600 check is encapsulated in a **modular child pipeline** for clean, reusable code.
- The parent pipeline seamlessly passes the `RecordCount` as a parameter through the entire chain of child pipelines, demonstrating advanced orchestration.

📸 **Evidence:**

- ✅ **Main Orchestration Pipeline:** The complete `PL_Ingest_Customers_To_Synapse_Incremental` pipeline showing the end-to-end flow.

  ![Main ELT Orchestration Pipeline]([LINK_TO_SCREENSHOT_07])

- ✅ **High-Performance `COPY INTO`:** The script activity using the `COPY INTO` command with a dynamic filename variable.

  ![High-Performance Loading with COPY INTO]([LINK_TO_SCREENSHOT_09])

- ✅ **Modular Child Pipeline Call:** The main `If` condition calling a child pipeline and passing the record count as a parameter.

  ![Modular Conditional Logic via Child Pipelines]([LINK_TO_SCREENSHOT_11])

- ✅ **Successful End-to-End Run:** The full pipeline completed successfully, including the conditional execution path.

  ![Successful End-to-End Run of the Main Pipeline]([LINK_TO_SCREENSHOT_12])

---

### 3️⃣ Enterprise-Grade Architecture & Governance 🏛️

**Objective:** Build the platform on a foundation of security, governance, and modern software development practices.

#### Implementation Details:
- The **Medallion Architecture** was implemented to ensure data quality, promoting data from a raw **Bronze** layer to a cleansed **Silver** layer via a stored procedure.
- A full **CI/CD workflow** was established using **Git integration**, with development in feature branches and deployment from `main` via Pull Requests.
- **Microsoft Purview** was integrated to provide **automated data lineage**, allowing anyone to trace the flow of data from source to destination.

📸 **Evidence:**

- ✅ **Overall Architecture:** All Azure services deployed in a single, managed resource group.

  ![Overall Project Architecture]([LINK_TO_SCREENSHOT_01])

- ✅ **Silver Layer Logic:** The `sp_ProcessStagingCustomers` stored procedure containing the UPSERT and data quality logic.

  ![Silver Layer Transformation Logic in Synapse]([LINK_TO_SCREENSHOT_10])

- ✅ **Final Verified Output:** The `Silver.Customers` table populated with clean, analysis-ready data.

  ![Final Verified Output in the Silver.Customers Table]([LINK_TO_SCREENSHOT_13])

- ✅ **Automated Data Governance:** The lineage graph in Microsoft Purview showing the automated tracking of data flow from ADLS to ADF.

  ![Automated Data Governance with Microsoft Purview]([LINK_TO_SCREENSHOT_14])

---

## ❗ Challenges Faced & Solutions

This project was a journey of solving real-world engineering problems. The key challenges were:

- **Synapse T-SQL Dialect Quirks:** 🚧 Encountered multiple syntax limitations in Synapse Dedicated SQL Pools.
  - 👉 **Solution:** I systematically debugged each issue, refactoring the SQL scripts to use Synapse-compatible patterns like `IF EXISTS...DROP/CREATE` and explicitly creating log tables as `HEAP`.
- **ADF Dynamic Expression Bugs:** 🐞 The `Copy Data` activity's output did not reliably provide the filename for downstream use.
  - 👉 **Solution:** I engineered a robust "Set Variable" pattern to generate a unique filename at the start of the pipeline run and pass it as a variable, making the flow predictable and reliable.
- **Nested `If Condition` Limitation:** 👨‍💻 Discovered that ADF does not support directly nesting `If Condition` activities.
  - 👉 **Solution:** I refactored the design to use a chain of modular child pipelines. This not only solved the problem but resulted in a cleaner, more scalable, and professional architecture.

---

## 🧠 Key Learnings

- **End-to-End Platform Design:** I mastered the integration of ADF, Synapse, ADLS, Key Vault, and Purview to build a complete and cohesive data platform.
- **Advanced Orchestration & Patterns:** I implemented complex conditional workflows, multi-level parameter passing, the Medallion Architecture, and high-performance ELT patterns.
- **Enterprise Best Practices:** I successfully applied critical concepts like CI/CD with Git, centralized secret management with Key Vault, and automated data governance with Purview.
- **Resilient Debugging:** This project taught me the immense value of reading error messages carefully, understanding system limitations, and being willing to pivot my design to find the most robust solution.

---

## 🚀 Future Enhancements

This platform provides a strong foundation that can be extended with several key data-centric features:

1.  **Implement the Gold Layer:** 🏆
    The next logical step is to build aggregated tables (data marts) on top of the `Silver.Customers` data. For example, creating a view or table that summarizes `Monthly Active Customers` by region.This pre-calculated data is highly optimized for business intelligence tools like Power BI, enabling faster dashboards and reports for business users.

2.  **Proactive Data Quality Monitoring:** 🔔
    Enhance the existing Stored Procedure to move records that fail data quality checks (e.g., have a NULL email or an invalid country code) into a separate `Quarantine` table. An ADF activity could then check the count of this table.This creates a system for actively monitoring and managing data quality issues. If the quarantine table has data, an alert can be sent to the data team to investigate problems at the source.

---


## 🙏 Acknowledgements

A huge thank you to my mentor, **[Mentor's Name]**, for their invaluable guidance, feedback, and for pushing me to explore these advanced concepts. This project would not have been possible without their support and the incredible learning opportunity provided by **[Company/Internship Program Name]**.gram Name]**.
