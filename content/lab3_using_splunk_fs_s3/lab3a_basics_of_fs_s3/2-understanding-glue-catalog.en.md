Now that we understand more around CloudFront access logs and have seen them being generated in Amazon S3, it is time to explore our first AWS Glue data catalog.

## Exploring AWS Glue ##

1. Let's get started by first navigating to the AWS Glue service. In the AWS Console, **search for and select AWS Glue.**

![Select AWS Glue from the AWS console](/static/lab3/3a/selectawsglue.png)

>[!NOTE]
>**AWS Glue** at its heart is an AWS Extract, Transform and Load (ETL) service. There are a lot of features and options around this service we will not cover off in this workshop as we are primarily focusing on the Glue Data Catalog capabilities.


2. Welcome to the AWS Glue service. As mentioned above we will be focusing on the Data Catalog features of the AWS Glue service. To begin, **select** the **Databases** link on the left-hand menu **under Data Catalog**.

![Select the Databases link on the left-hand menu under Data Catalog](/static/lab3/3a/gluedatabaselinkselect.png)

3. **Select the splunk-fs-s3-{account-id} database** link as shown below. 

![Select Splunk Glue Database that has already been created](/static/lab3/3a/select-splunk-glue-database.png)

>[!NOTE]
>**Glue Table Information:** You will note with any glue table it shows you high level information of the name, the database its associated with and also the S3 location where the schema is pointing too.


4. Now that we are in the database we see the tables associated with the database. At this point in time there is only one, splunk_cloudfront_access_logs_w3c. It's time to explore this table further. **Click the splunk_cloudfront_access_logs_w3c table** as shown in the screenshot below.

![Select CloudFront Glue Table from within the database properties](/static/lab3/3a/click-cloudfront-glue-table.png)

5. From here there are specific details on the table which was created automatically as part of the lab setup earlier. Let's **click the Schema tab** as shown below

![Select CloudFront Table Schema Tab](/static/lab3/3a/select-schema-tab-glue-cloudfront-table.png)

>[!NOTE]
>From here you can see important information on the schema structure of the CloudFront Access Logs data. Specifically for w3c format which was the default for the CloudFront Access logs. If another format type was selected, e.g. json, then this schema would be different to match the required json format.


## Compare Log to Schema ##
Let's now compare the schema to the log file itself and see what we notice.

1. From the AWS console. Search for and **open in a NEW TAB S3**

![Select S3 from the AWS console](/static/lab3/3a/selects3.png)

2. **Select the bucket for the CloudFront Access logs** (e.g. splunk-fs-s3-logs-{account-id}-{region})
3. Like before **select the folders until you get to the log folder for CloudFront.** See example below

![View CloudFront Logs bucket that has been created in AWSLogs/](/static/lab3/3a/viewcloudfrontlogsins3.png)

4. **Select the tick box for the first file** in the folder, then **select the Download button** to download the file to your computer. See example below.

![Download CloudFront Log from S3 ](/static/lab3/3a/download-cf-access-log-from-s3.png)

5. Using any file browser **go to the location of where the file was downloaded.**  
6. **Open the log file** which is inside the .gz file you downloaded. It should look like the example image below.

![Sample CloudFront Access Log that you will see when you open the file that you have downloaded](/static/lab3/3a/sample-cloudfront-log-format.png)

7. **Note the header fields of the column #Fields**. Do they look familiar? They should. They are the same structure as the schema in AWS Glue table we looked at earlier. **Compare the two** and move on when you are finished.

6. Before we move on feel free to explore any other information in the table. e.g. the Advanced Properties tab. These tend to include any specific information relevant to the structure of the data.

**Congratulations!** Now that you are familiar with an AWS Glue Database and schema it's time to create our first Splunk Federated Index and run a basic search from Splunk on the CloudFront Access Logs.

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3a_basics_of_fs_s3/3-creating-out-first-fs-s3-index.en.md)</a> to begin! 

