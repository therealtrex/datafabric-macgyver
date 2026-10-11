# Routing Filtering and Routing Cisco ASA Firewall Logs

## Scenario Overview ##
As explained during the architecture overview, the team at `imnotbiased.com` would like to route the raw Cisco ASA firewall logs to Amazon S3 to long term archiving. The team would also then like to send a subset of these logs into Splunk for security use cases. 

---

### Implementation Steps ###
To address our required we will:

1. **Setup Amazon S3 destination
   * Configure the data management platform to have Amazon S3 bucket as a destination. 

2. **Clone the Original Feed:**
   * Route the raw Cisco ASA logs to an Amazon S3 bucket we create in our lab.

3. **Create a pipeline to filter Cisco ASA logs:**
   * Crete an Ingest Processor pipeline to filter our unwanted data by the security team.
   * Route this data into the intended Cisco ASA index for security use cases.

---

>[!NOTE]
>Because we are keeping the raw logs in S3 we can always use federated search for amazon S3 later on to search those logs again if required. This might help if we want to check something or change the pipeline to include more fields if required. 
>Also note, like lab 1, this lab can be implemented using either **Edge Processor** or **Ingest Processor**. In fact technically Edge Processor could be a better fit as routing data within the customer's Amazon tennancy would reduce on potential egress costs. 
>For simplicity though, Ingest Processor is used throughout this lab.

<br>

### Click  <a>[next](/content/lab2_cisco_firewall_logs/01_configuring_amazon_s3_destination.md)</a> to start the lab 