In this section of lesson two we will have a look at Amazon Athena and its powerful Data Definition Language (DDL) which can be used to create AWS Glue Tables. We will see this first-hand by creating a new table for our VPC Flow logs we created in the previous section.

## Amazon Athena DDL ##

1. Let's start by navigating to the Amazon Athena service. In the AWS Console, **search for and select, Athena.**

![Select Athena Service in the AWS console](/static/lab3/3b/select-amazon-athena-service.png)

2. From the main Athena welcome page we need to **select Query your data in Athena console** option followed by **Launch query editor.** See example below: 

![Welcome Amazon Athena Page Selection](/static/lab3/3b/welcome-athena-page.png)

3. Welcome to Athena Editor! From here you should be taken straight into the **Editor tab. If not select it now.**

>[!NOTE]
>If you see a screenshot like the one below, then you may need to configure the S3 bucket for Amazon Athena. It is required for running queries. Follow the optional steps below it fix this issue.
>
>![Athena S3 Result Bucket Missing Prompt is shown](/static/lab3/3b/fix-athena-s3-results-bucket-prompt.png)


<details>
<summary>Fix Athena Query Result Location</summary>
<p>1. From the blue box shown above **select Edit Settings**</p>
<p>2. **Click the blue Manage button on the right hand side** in the query result section. </p>
<p>3. **Click the Browse S3** blue button on right-hand side.</p>
<p>3. **Select the splunk-fs-s3-workshop-athenaoutput-xxx s3 bucket.**</p>
<p>4. Once selected, **click the Choose button.**</p>
<p>5. Leave all remaining settings and **click Save**</p>
<p></p>
<p><img src="/static/lab3/3b/athena-query-result-bucket-settings.png" alt="Alt text"/></p>
<p></p>
<p>6. You should be prompted with Settings successfully updated green box. You will note you are in the Settings tab now. **Click Editor Tab** to go back to the query editor section of Athena.</p>
<p></p>
</details>

>[!NOTE]
>Before we dive into this Amazon Athena DDL setup for VPC Flow logs, I wanted to quickly call out what we are setting up.
>
>Amazon Athena uses a language known as DDL (Data Definition Language) which is a subset of SQL commands. We can use DDL for many things like querying data but for this example we will use it to create a Amazon Glue Table for our VPC flow logs. This is a great place to start when first learning how to create Amazon Glue tables and there are plenty of examples online for different AWS sources. 


4. Time to run our first Amazon Athena query. On the left-hand pane under Data, Make sure **database is set to our splunk-fs-s3-{account-id} database.**

![Select Glue Database in Athena under editor](/static/lab3/3b/select-glue-database-in-athena.png)

5. Before we can run our first Athena query we need our S3 location for our VPC flow logs. If not open already **in a NEW TAB go to Amazon S3** and **select your S3 logging bucket e.g. splunk-fs-s3-logs-{account-id}-{region}**
6. Click into the bucket until you see the aws-service=vpcflowlogs folder. **Select the tick box next to it** and then **click the button for Copy S3 URI**. See example image below. 

![Copy S3 bucket URI for vpcflowlogs using the blue button on the left hand side](/static/lab3/3b/copy-vpcflowlogs-s3-uri.png)

7. **Paste this somewhere to use in next steps** It should be something like this: **s3://splunk-fs-s3-logs-{account-id}-{region}/AWSLogs/aws-account-id={account-id}/aws-service=vpcflowlogs/**

8. Back in our Athena tab, **Copy and paste the following code into the Query editor box.**
  

```Athena Create VPC Flow Log Table
CREATE EXTERNAL TABLE IF NOT EXISTS splunk_vpc_flow_logs (
  version int,
  account_id string,
  interface_id string,
  srcaddr string,
  dstaddr string,
  srcport int,
  dstport int,
  protocol int,
  packets bigint,
  bytes bigint,
  start bigint,
  `end` bigint,
  action string,
  log_status string,
  vpc_id string,
  subnet_id string,
  instance_id string,
  tcp_flags int,
  type string,
  pkt_srcaddr string,
  pkt_dstaddr string,
  region string,
  az_id string,
  sublocation_type string,
  sublocation_id string,
  pkt_src_aws_service string,
  pkt_dst_aws_service string,
  flow_direction string,
  traffic_path int
)
PARTITIONED BY (
  `aws-region` string,
  `year` string, 
  `month` string, 
  `day` string,
  `hour` string
)
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
STORED AS INPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION
  '<S3 URI>'
TBLPROPERTIES (
  'EXTERNAL'='true', 
  'skip.header.line.count'='1'
)

```
11. Scroll down the bottom of the DDL and **fill out the location section labelled \<S3 URI\>** using the S3 URI we copied earlier. Pay careful attention to the format when copying it. 
- eg s3://splunk-fs-s3-logs-{account-id}-{region}/AWSLogs/aws-account-id={account-id}/aws-service=vpcflowlogs/

12. Feel free to explore the DDL code, you will note right at the top is the name of the new table we are creating. When ready, **click the Run button.**
15. If all goes to plan you should see a **Green Completed sign below** the Run button. See example below

![Athena text being run that shows a DDL Query Successful](/static/lab3/3b/ddl-query-successful.png)

Next we need to tell Athena to run a repair command to refresh and detect the partitions in the folder structure. 

16. In AWS Athena, **click the '+' button on the right hand side** to create a new Athena query. 

17. **Paste the following code in below and click Run**

>[!WARNING]
>Before running the command make sure your database is set to splunk-fs-s3-{account id} as Athena changes it to default after running the first query above!

```Athena Repair Command
MSCK REPAIR TABLE splunk_vpc_flow_logs
```

>[!NOTE]
>You should see a completed green repair command noting also it tells you the partitions which weren't in the metastore and were added to the table.  If you do not see this then you may not have set the database correctly as mentioned above. See example below: 
>
>![Successful Repair VPC Flow Logs Table](/static/lab3/3b/sucessful-athena-repair-command-vpcflowlogs.png)


17. Now let's check out our new AWS Glue Table. From your AWS Console, **search for and open in a NEW TAB, AWS Glue.**

![Select AWS Glue from the AWS Console](/static/lab3/3b/selectawsglue.png)

18. From your AWS Glue page, **select Databases**
19. **Select the splunk-fs-s3-{account-id}** from the list
20. You should now see two tables. Once from Lesson One for CloudFront, and the new one we just created. **Select the splunk_vpc_flow_logs table.**

![Select AWS Glue VPC Flow Log Table](/static/lab3/3b/select_vpc_flow_logs_glue_table.png)

21. From here it is now time to **compare the schema settings with the VPC Flow log you opened up** in section one of lesson two. Have a look at the headings and structure of the schema. They should match! See example below:

![Example text showing to compare VPC Flow Log File to Glue Table Schema](/static/lab3/3b/compare-vpc-flow-log-to-glue-table-schema.png)

>[!NOTE]
>You may see additional fields in the Glue table schema which aren't in your VPC Flow log, e.g. vcp_flags and pkt-srcaddr. This is ok; These additional fields may appear in more advanced VPC flow lot types so important to define for those use cases in the future.

22. Feel free to click **Advanced Properties** to look at the additional settings before moving on. These settings were also configured during the DDL command.

Well done! You have now configured your first Glue Table using Amazon Athena DDL script. This is a powerful tool and very handy way to create AWS Glue Tables easily. Time to now create another Federated dataset in Splunk!

**Congratulations!** That ends the second section of lesson two; configuring an AWS Glue table using Amazon Athena DDL. You should now have deployed your first Glue Table using DDL and was able to confirm the structure of the schema was consistent with the log file. Now it's time to configure another Splunk Federated dataset to search the VPC Flow logs in real-time.

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3b_using_athena_ddl_for_aws_glue/3-configure-federated-index-splunk-vpc-logs.en.md)</a> to continue. 
