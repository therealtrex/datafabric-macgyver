## Section Three: Routing Cisco ASA data to Amazon S3 and Filtering Logs

In this final section of our lab we will create an Ingest Processor pipeline to split a raw copy of the logs and send them to our new destination of S3. Then we will create the second part of our pipeline to filter our unwanted data from the Cisco firewall logs to reduce our ingestion size. 

Ok, let's get started!

### Creating Ingest Pipeline to route to Amazon S3. 

1. From your SCS tenancy tab, `click the Splunk cloud logo` top left-hand side. 
2. From here we want to `click on pipelines` on the left-hand menu

From here we should still see our first pipeline we created in lab 1. It is time now to create a new pipeline. 

3. `Select the blue + New Pipeline` button on the top right-hand side. 
4. `Select Ingest Processor pipeline.` See example below 

![image_tag](/static/lab2/select_ingest_processor_pipeline_creation.png)

5. From our getting started page again we want to `select Blank pipeline`

6. `Click next when ready`

Remember what we said around partitions before? Well this time we will try something different. This time we know our Cisco logs are all coming in with a sourcetype of cisco:asa. So this seems to be a good partition type. 

7. From our pipeline partition page, `select partition by sourcetype.` 

8. For our value, `copy and paste the following sourcetype.` See example of it filled in below:

```text
cisco:asa
```

![image_tag](/static/lab2/partition_by_sourcetype_cisco_asa.png)

9. `Click Apply` when ready. 

10. Check it looks ok and `click next` when ready. 

Next is our sample capture again. Seeing as we have live data coming in let's again use the Capture new snapshot option. 

11. `Click the + Capture new snapshot` button on the right-hand side. 

12. `Copy and paste the text below into the Name field.`

```text
cisco_asa_snapshot
```

13. `Leave defaults and click Capture.`

14. Makes sure the new capture is selected. `Click next` when ready. 

You will notice in our destination page we will see our new destination for Amazon S3. See example below: 

![image_tag](/static/lab2/new_s3_destination_in_pipeline_wizard.png)

15. `Select our new cisco-asa-s3-logs destination.`

16. `Click Done` when ready. 

You should now see a default blank pipeline ready to go! Hopefully by now you are getting a grip on what is what and what will happen if we apply it now? 

Well spoiler alert. it will route the entire feed to Amazon S3 removing it from Splunk all together! 

That's not quite what we want. So let's just initially clone it into two. One for S3 and one for Splunk still. 

### Cloning Cisco ASA logs into Amazon S3 destination and Splunk ###

1. From our pipeline `copy and paste the following code in replacing the entire code block.`

```
import 'cisco-asa-s3-logs' from ~/federated

$pipeline = | from $source
| branch
    [
        // Branch 1: Route to Splunk Cloud
        | eval index = "cisco-asa-logs"
        | into $destination
    ],
    [
        // Branch 2: Route copy to Amazon S3
        | into 'cisco-asa-s3-logs'
    ];
```

See example of pipeline below: 

![image_tag](/static/lab2/clone_data_s3_cisco_asa.png)

<br>

We should also check the left hand side settings to make sure they look correct as well. See example below:

<br>

![image_tag](/static/lab2/initial_split_cisco_asa_piepine_right-hand-side.png)

Its time to save and test our output. 

2. When ready, `click the Save pipeline button` top right-hand side. 

3. `Copy and paste the name below.`

```text
cisco_asa_pipeline
```

4. Add optional description and `Click Save when ready.`

5. When prompted, `click Yes, apply`

Once its saved, let's validate it looks ok from the Ingest Processor perspective. 

6. `Click the Splunk logo` top left-hand side. 

7. `Select pipelines` on the left-hand menu. 

Hopefully now you see two pipelines in your list and both of them are green and applied! See example below: 

![image_tag](/static/lab2/cisco_asa_pipeline_version1_applied.png)

its time to validate if its working. Let's check Splunk first and make sure we haven't broken anything! 

### Validate version one of Cisco ASA Pipeline ###

1. From your Splunk console (not SCS!), `Click the Splunk Cloud logo` on the top left-hand corner. 

2. `Select the App for Search and Reporting`

3. `Set the time range picker for last 15 minutes`

4. Copy and paste the following SPL and run the search. 

```
index="cisco-asa-logs"
```

Hopefully we can still see current data coming into Splunk. Feel free to run it a few minutes later or so just to confirm! See example below: 

![image_tag](/static/lab2/validated-logs-still-in-splunk-cisco-asa-pipeline-v1.png)

Now that we have confirmed Splunk is still receiving  logs and looks like normal. Let's check our AWS S3 bucket! 

5. From our AWS console. `Search for and Select S3`
6. `Select our S3 bucket we created earlier,` eg splunk-cisco-asa-s3-log-storage-<yourname>

We should now see under options a folder created by ingest processor called sourcetype=cisco%3Aasa/

7. `Navigate into the folders until we fine our first set of parquet logs.` See example below. 

![image_tag](/static/lab2/cisco-asa-parquet-logs-v1.png)

>[!NOTE]
>Ingest Processor created hive partition folders by default. Eg host=xyz, year=2026 and month=10 etc etc. This is handy for running federation searches later and makes it easier to find current logs too. 

Looks like everything it working. let's just try a quick additional check! 

8. `Select any log file tick box and click the download button.` See example below:

![image_tag](/static/lab2/download-parquet_file_s3.png)

9. Go to the following website and upload the file to check it out! 

<a>[Parquet File Reader](https://parquetreader.com/)</a>

See example image of opened log file for Cisco ASA:

![image_tag](/static/lab2/read-parquet-file-cisco-asa.png)

Excellent! So the first part of our pipeline is working. We have sucessfully clones to copies of the logs. One to Splunk and one to Amazon S3. Its now time to modify our pipeline again in the next section to filter our unwanted Cisco ASA fields. 

### Ingest Processor Pipeline to Filter our Cisco ASA Fields ###




