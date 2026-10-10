## Section One: Viewing Our Existing Data

Let's start off by inspecting the existing credit card transactions currently being ingested into the Splunk platform.

### Step-by-Step Instructions

1. **Open Search & Reporting:**
   * From your Splunk Web home page, select **Search & Reporting** from the left-hand navigation menu.

    ![image_tag](/static/lab1/select-search-and-reporting.png)

2. **Run Initial Discovery Search:**
   * In the search bar, keep all default settings and execute the following SPL search:
     ```spl
     index="cc_trans_im"
     ```

    ![image_tag](/static/lab1/sample-cc-trans-unfiltered.png)

   * Set the time range picker to **Last 15 minutes**.

3. **Inspect the Dataset:**
   * Spend a few minutes reviewing the returned events. 
   * Notice that unmasked PII (such as full credit card account numbers and sensitive customer details) is visible in plain text.

---

> **Key Takeaway:** Now that we have identified the exposed PII in the current data stream, we will use **Ingest Processor** to filter and mask this data before storage.