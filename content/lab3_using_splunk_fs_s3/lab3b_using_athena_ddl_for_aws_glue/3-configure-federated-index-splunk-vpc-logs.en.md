## Creating a Second Splunk Federated Dataset ##

>[!WARNING]
>This lesson assumes you have completed Lesson One completely as we will be modifying in Splunk the existing Federated Provider and Creating a new Federated Dataset for VPC Flow logs. If you have not completed Lesson One. Feel free to do that now and come back to this section.


In this lesson we will configure a new Splunk Federated Dataset for VPC Flow logs.

1. `Login to your Splunk instance if it has expired` and navigate to the Data Management app located on the left hand menu. See example below:

![Select Data Management App Splunk Cloud](/static/lab3/3b/selecting-data-management-app-in-splunk.png)

2. From here we should be taken into the Data Management App. `Click datasets` on the left hand side. 

3. We should see our existing datasets still there. `Click Create Dataset` on the right hand side. 

4. `Select Amazon S3 and Click Next`

5. `Select the existing connection and Click next` when ready. 

6. Copy and paste the name below into the Dataset name field. 

```
vpc_flow_logs
```

7. `Paste the same S3 location we used earlier in our DDL command for the Amazon S3 location` eg s3://splunk-fs-s3-logs-{account-id}-{region}/AWSLogs/aws-account-id={account-id}/aws-service=vpcflowlogs/

8. `Select Federated search radio button.` See example below for filled out example:

![Filled Out Dataset Definition](/static/lab3/3b/filled-out-dataset-vpc-flow-logs.png)

9. `Click next` when ready

10. `Select AWS Glue` from the radio list for catalog options. 

>[!NOTE]
>Before we move on we need to open up our AWS Glue table (if not open already) to get information ready for the next step. 


11. From your AWS console. `Search for and select AWS Glue.`

![Select AWS Glue in the AWS Console](/static/lab3/3b/selectawsglue.png)

12. From the left-hand menu `select Tables.`

>[!NOTE]
>You may have to select the Splunk table from the choose database section on the next page.

13. `Select the new table we created earlier e.g. splunk_vpc_flow_logs`

From here you will note the key fields we will use in the next Splunk screen. 

12. Like in lesson One, `fill in the AWS Glue Catalogue id (this is the AWS account number), AWS Glue Database and AWS Glue Table name` which are in the previous AWS Glue Table overview screen. See example below (note the AWS account-id is blacked out)

![Filled out catalog settings VPC Flow Logs](/static/lab3/3b/filled-in-catalog-settings-vpcflowlogs.png)

13. `Select Non-table format` from the list

14. `Toggle the define the time field` option to turn it off. 

15. Leave remaining defaults, scroll down and `click next` to continue. See example below: 

![Filled out remaining catalog settings VPC Flow Logs](/static/lab3/3b/fill-in-remaining-catalog-settings-vpcflowlogs.png)

16. `Select no for KMS keys` option. 

17. `Click generate policies.` Please review the policies and make sure they make sense to you before moving on. 

>[!NOTE]
>You should have noticed updated policies for the VPC flow logs in the glue table settings and S3 bucket location. Normally these would need to be appended to the existing policies we used for Lesson One but the lab environment has used partial matches for its policies so we do not need to update them.  

18. `Click Next` when ready

19. `Click confirm` to acknowledge policy warning 

20. Review items and `click Create Dataset` when ready.

>[!NOTE]
>A blue box should appear notifying us that the new dataset has been created and is ready to be searched. So let's go try that. Note, the new dataset may take a few minutes to be actually ready. But don't worry we point that out if you do try and search to quickly. 


## Testing out new VPC Flow Log Federated Index ##

Its time now to run a Splunk search against our new VPC Flow log Federated Dataset. 
1. From the Splunk console. `Click the 9 set of square dots on the top right hand side, followed by the Search & Reporting link`. See example below 

![Select search and reporting in Splunk](/static/lab3/3b/select-search-and-reporting.png)

2. From the search bar, `select the SPL drop down and choose the SPL2 option` as shown in picture below:

![Select SPL2 from Search bar](/static/lab3/3b/select-spl2.png)

3. Using the code below, `copy and paste into the search bar.` You may need to adjust the time settings to something larger. e.g. All time. 

```SPL2
index=vpc_flow_logs | head 100
```

4. Once completed you should see a result similar to the below. 
![Successful Splunk VPC Flow Logs returned in the Splunk search console](/static/lab3/3b/vpcflowlogs-search-returned-splunk.png)


**Congratulations!** on configuring VPC Flow logs to work with Splunk Federated Search for Amazon S3. You have picked up a few new skills in this lab. 
- You have learnt how to set up VPC flow logs and Deployed an EC2 instance to generate logs.
- You have learnt how Amazon Athena can be used to run DDL queries to create Glue Tables. 
- You have experienced your first set of data in Apache Hive format.

Next Lesson we will explore AWS WAF Logs with FS-S3 and use a new way configuring them in Splunk, using the Splunk managed crawler method. 

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3c_spunk_managed_catalog/index.en.md)</a> to continue. 