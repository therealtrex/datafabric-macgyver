## Section Two: Creating Amazon S3 bucket and configuring Splunk data connection

Now we have successfully created a connection to Amazon in our Data Management app. We can continue onto the next phase which is to create an Amazon S3 bucket and then configure a dataset in Splunk data management to be able to route to that bucket.  

### Creating an Amazon S3 bucket for Cisco ASA ###

1. From your AWS console. `Search for and select Amazon S3.` See example below:

![image_tag](/static/lab2/search-and-select-amazon-s3.png)

2. From the Buckets screen. `Click the Create Bucket` button near the middle of the screen.

>[!NOTE]
>There is a quite a lot to know about using Amazon S3. This workshop will not go into any of the details about it but for this lab all we will say is it is a great low cost storage service which provides near limitless storage and hugely available up time and redundancy. More information on Amazon S3 can be found <a> [here](https://aws.amazon.com/s3/)</a>

3. Find the section where it says **Bucket Name**. `Copy and paste the below bucket name, appending your name at the end`

>[!IMPORTANT]
>You must have `splunk-` at the start of your bucket name. Otherwise the permissions we have configured as part of the role in this workshop will not allow you to write and read files from it. Please use the suggested format in the lab for S3 bucket name. 

```
splunk-cisco-asa-s3-log-storage-<yournamehere>
```

4. `Leave all remaining defaults`, scroll to the bottom and `click Create Bucket.`

You should now see a screen similar to the below:

![image_tag](/static/lab2/successfully-created-s3-bucket-cisco-asa.png)


We can leave the Amazon S3 tab open and jump back into Splunk as for our next step we need to create a dataset which we will use for routing data too. 

### Creating a Splunk dataset for Cisco ASA logs ###

5. From the Splunk console, if not already open go to the Data Management app by `clicking the Splunk Cloud logo followed by the Data Management app` on the left-hand menu. 

6. From here we will now go to a new section. `Click Datasets` option on the left-hand menu. See example below:

![image_tag](/static/lab2/select-datasets-data-managment-app.png)

From here we will already see a default dataset which is configured for Machine Data lake.. More on that later!

7. When ready, `click the blue Create dataset` button on the top right-hand side. 

>[!NOTE]
>WOW! Look at how many different options we have here for dataset creation. This is the list of all of the different types of data Splunk is able to route and search from today, with more coming in the future. Isn't that cool!

8. For this lab we will be using Amazon S3 again. `Select the Amazon S3 option` as shown below: 

![image_tag](/static/lab2/select-amazon-s3-dataset-option.png)

9. When ready, `click the next` button. 

10. `Select our connection we created earlier` in the previous part of lab2 from the drop down. 

11. Feel free to review and `click next` when ready. 

This is where we can define the dataset type again between searching and writing data to it.

12. `Fill in the options as mentioned below.` See example filled out below: 

- Dataset name: `cisco-asa-s3-logs`
- Dataset Description: `Dataset for CISCO ASA Logs in S3`
- Amazon S3 location: `s3://cisco-asa-s3-log-storage-<yourname>` 
- Usage Radio buttons: `Data routing and federated search`

<br>

![image_tag](/static/lab2/filled-out-dataset-definition-page.png)

<br>

13. When ready, `click next` to continue.

Now, this is where things really get exciting! There are lots of different options and nuances here. We cannot cover off all options apart from what we are going to use in the lab. For more detailed information on configuring Splunk datasets. See <a>[HERE](https://help.splunk.com/en/splunk-cloud-platform/process-data-at-ingest-time/use-ingest-processors/send-data-out-from-ingest-processor/send-data-from-ingest-processor-to-amazon-s3/create-an-amazon-s3-dataset-for-ingest-processor-pipelines)</a>

14. Fill out the Configure dataset options as described below:

- Output schema: `Pipeline output`
- Output format: `Parquet`
- Compression type: `uncompressed`
- File name prefix: `leave blank`
- Advanced settings: `leave as is`
- Federated Search Activate: `toggle NO` 

>[!NOTE]
>Some quick notes on what we selected and why for this workshop. 
>
>`Output schema:` Pipeline output was selected as we are using this dataset for Ingest Processor and this is the best option for that
>
>`Output format:` Parquet was selected as this is a binary format which is great for reducing storage size on S3 especially for long term rention. Federated search can also search parquet. Note though if you want to manually ingest into Splunk later on using a PULL (eg Splunk Add-on for Amazon S3) then the parquet format will need to be converted to JSON. Keep this in mind in your onw scenario. 
>
>`Compress Type:` Selected uncompress as we wanted to quickly and easily show you the results in S3
>
>`File name and prefix:` You can add a prefix to the parquet output filename. For this lab we won't need it. 
>
>`Federated Search Activate:` Although we could use federated search as a good use case here for this lab, we will be doing very similar capabilities on other labs later on so to save time and confusion we will leave it to those labs later on. So we turned it off for this lab. 

See example of filled in page below: 

![image_tag](/static/lab2/configure-dataset-options-s3.png)

15. Review and when ready, `click next`

16. `Select no` on the KMS key screen. We are not using KMS keys for this lab. ALthough you probably should in a production environment! 

17. `Click Generate policies` button. 




