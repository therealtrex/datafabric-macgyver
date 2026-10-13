# Machine Data Lake Ingestion and Promotion of Data 

## Scenario Overview ##
Not all data is equal. A bunch of web access logs are coming into the Splunk platform and the team does not want to store these in the speedy Splunk dynamic data active search (DDAS) tier. They heard about Splunk's new Machine Data lake which is a super low cost storage capability which allows you to dump data in, search it then when/if you choose, promote it to a more performant tier like Analytics table or DDAS. 

In this lab we will route some data from DDAS to Machine Datalake Raw table. Once there we will search it and then run a promotion of some of it back into DDAS showing the flexibility of the platform. 

---

### Implementation Steps ###
- Check the data is coming into Splunk
- Create a Machine Data Lake raw table for ingestion of Linux_secure logs.
- Route data away from DDAS into new MDL raw table. 
- Validate the data is no longer going to DDAS and is indeed going to MDL
- Lastly we will then promote a small set of data back into DDAS again.
  
---

Ok, let's do this! 

### Click  <a>[next](/content/lab4_machine_data_lake_101/01_create_mdl_dataset.md)</a> to start the lab 