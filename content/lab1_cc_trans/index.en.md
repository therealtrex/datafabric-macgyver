# Cisco Data Fabric MacGyver Lab1: PII Credit Card Transactions

## Scenario Overview ##
As explained during the scenario walkthrough, the team at `imnotbiased.com` needs to mask Personally Identifiable Information (PII) in credit card transaction details. Currently, all staff using Splunk are required to take PII training, and compliance auditors have flagged this access level as an active business risk.

---

### Solution Strategy ##
To resolve this issue, we will use **Ingest Processor** to split and route the data stream into two separate target indexes:

* **Index One (Raw / Audit):** Contains the unfiltered, raw data. Access is strictly restricted to specialized audit and compliance personnel.
* **Index Two (Masked / Monitoring):** Stores sanitized, masked, and filtered credit card transaction data for the infrastructure monitoring team.

---

### Implementation Steps ###
To address the Credit Card Transactions use case:

1. **Clone the Original Feed:**
   * Route the raw credit card transaction data to the new restricted PII index.
   * **Target Index:** `cc_trans_pii`

2. **Mask and Filter PII Data:**
   * Apply data-masking rules to redact PII from the credit card transactions.
   * Route the sanitized data to the credit card infrastructure monitoring index.
   * **Target Index:** `cc_trans_im`

---
>[!NOTE]
>This lab can be implemented using either **Edge Processor** or **Ingest Processor**. For simplicity, Ingest Processor is used throughout this lab.

<br>

### Click  <a>[next](/content/lab1_cc_trans/01_section_one_viewing_existing_data.md)</a> to start the lab 