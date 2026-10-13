# Machine Data Lake Ingestion and Promotion of Data 

## Scenario Overview ##
Not all data is equal. A bunch of web access logs are coming into the Splunk platform and the team does not want to store these in the speedy Splunk dynamic data active search (DDAS) tier. They heard about Splunk's new Machine Data lake which is a super low cost storage capability which allows you to dump data in, search it then when/if you choose, promote it to a more performant tier like Analytics table or DDAS. 

In this lab we will route some data from DDAS to Machine Datalake Raw table. One there we will search it and then run a promotion of some of it back into DDAS showing the flexibility of the platform. 

---

### Implementation Steps ###
To address our requirements we will:

1. Setup Amazon S3 destination by Configuring the data management platform to have Amazon S3 bucket as a destination. 

2. Clone the original Cisco ASA data feed and route the raw Cisco ASA logs to an Amazon S3 bucket we create in our lab.

3. Lastly, create a pipeline in ingest processor to filter Cisco ASA logs.
   
---

>[!NOTE]
>Because we are keeping the raw logs in S3 we can always use federated search for amazon S3 later on to search those logs again if required. This might help if we want to check something or change the pipeline to include more fields if required. 
>
>Also note, like lab 1, this lab can be implemented using either **Edge Processor** or **Ingest Processor**. In fact technically Edge Processor could be a better fit as routing data within the customer's Amazon tennancy would reduce on potential egress costs. 
>
>For simplicity though, Ingest Processor is used throughout this lab.

<br>

### Click  <a>[next](/content/lab2_cisco_firewall_logs/01_configuring_amazon_s3_destination.md)</a> to start the lab 