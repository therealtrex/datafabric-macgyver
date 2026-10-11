## Section Two: Creating Our First Blank Ingest Processor Pipeline

### 1. Log In to Splunk Cloud Services (SCS)
1. Log in to your **SCS tenancy** using the link provided in your Splunk SHOW lab environment.
   > **Note:** The portal should automatically authenticate using your cached credentials from your Splunk Cloud session. If authentication fails, ensure you are still logged into the Splunk Cloud instance used in Section One.

---

### 2. Verify Ingest Destinations
Once logged in, you will see the unified data management overview:

1. In the left navigation menu, select **Destinations**.
   *(Placeholder: Screenshot - Destinations View)*
2. Confirm that `splunk_indexer` exists in the list. This pre-configured destination routes processed events into Splunk Cloud for indexing.

> **Callout - Unified Control Plane:** Notice the **Edge Processors** section in the navigation menu. Splunk Data Management provides a single centralized interface to configure and manage Edge Processors, Ingest Processors, and pipelines side by side.

---

### 3. Launch the Pipeline Creation Wizard
1. Select **Pipelines** from the left navigation menu.
2. Click the blue **+ New pipeline** button in the top right (or center) of the screen.
3. Review the available out-of-the-box pipeline templates for common data sources.
4. Select **Blank pipeline**, then click **Next**.

---

### 4. Configure Data Partitioning (Data Interception)
Splunk Ingest Processor supports four metadata partitioning criteria: `sourcetype`, `source`, `host`, and `index`.

> **Partitioning Strategy:** Because all credit card transactions are exclusively sent to one designated index (`cc_trans_im`) without mixed data streams, partitioning by **index** ensures we only process the target transactions without impacting unrelated feeds.

1. Select the **index** partition option.
   *(Placeholder: Screenshot - Partition by Index Modal)*
2. In the **Value** field, enter:
   ```text
   cc_trans_im
   ```