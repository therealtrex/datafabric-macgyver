In order to understand the benefit of VPC flow logs we need to configure some. VPC flow logs can be configured against many services. Even against an entire VPC, this provides an ability to be broad or granular. In this first part of lesson two we will set up VPC flow logs against a pre-deployed VPC in our workshop. We will then deploy an EC2 inside that VPC which when accessed will generate logs.

## Setup VPC Flow Logs ##

1. From your AWS console search for and `select VPC`

![Select VPC Service](/static/lab3/3b/select-vpc.png)

2. This is the VPC dashboard. From here there are many things to see and do but for this lesson we want to explore the VPC. From the `left-hand menu, select Your VPCs.`

>[!NOTE]
>There are many ways to configure VPC flow logs from the AWS Console. Below is the simplest for our workshop


3. From the VPCs page you will see the VPC created as part of the workshop. If it's not selected, select it now by ticking the tick box for it. See example below

![Select Your VPC](/static/lab3/3b/selectyourvpc.png)

4. This will open up (if not open already) a panel down below showing you key information about your VPC. Let's explore the resource map by `clicking the Resource Map tab.`

From here you will see the network layout of the VPC. This is a handy tool to check how things are configured for your VPC in a single visual representation. See example below

![Select Resource map in the top section](/static/lab3/3b/select-resource-map-tab.png)

5. Now it's time to create a VPC Flow log. `Select the Flow logs tab.` You will note currently there are no flow logs. See example below.

![Select Flow logs in the top section](/static/lab3/3b/select-flow-logs-tab.png)

6. On the right-hand side, click the Create flow log button

![Select Orange Create Flow Log Button](/static/lab3/3b/select-create-flow-log-button.png)

7. **Leaving this tab open** we need to go and copy the S3 ARN as we will need it to create a flow log. To do this `search for and open in a new tab S3.`

![Navigate to the S3 Service in the AWS console](/static/lab3/3b/selects3.png)

8. `Select the logging s3 bucket` for the workshop (same one used in lesson one), should be starting with **splunk-fs-s3-logs-{account-id}-{region}**

9. `Select the properties tab` and `copy the Amazon Resource Name (ARN) for the bucket`. See example below. Note: you can use the two blue boxes to copy it.

![Use the Copy button on the Logging Bucket S3 ARN](/static/lab3/3b/copy-s3-logging-bucket-arn.png)

10. Now we have copied S3 arn we will use that in the flow log setup page. `Go back to the other tab for your flow logs now.`

11. From the Create flow log page we will fill in a few details as outlined below. See example at end.
- **Name:** give your flow log a name e.g. full-vpc-flow-log
- **Filter:** leave as default (all)
- **Maximum aggregation interval:** Set to 1 minute
- **Destination:** Set to Send to an Amazon S3 bucket
- **S3 bucket ARN:** Paste the S3 bucket ARN you copied earlier

![Create Flow Log Settings with the detail as provided you will need to fill out several sections](/static/lab3/3b/create-flow-log-settings.png)

12. Scroll down to where it says `Log file format` and select `parquet` 
13. `Tick the box for Hive-compatible S3 prefix`
13. Select `Every 1 hour (60 minutes).` under partition logs by time. See example below for options selected:

![Partition by hour setting Flow Logs](/static/lab3/3b/logfileformat-partitionlogbytime-vpcflowlogssettings.png)

13. Leave all remaining settings and `click Create flow log`

14. You should see a green rectangle saying `Successfully created flow log for vpc-xxx.`

![Success Flow Log Created banner is green across the top of the screen](/static/lab3/3b/success-vpc-flow-log-created.png)

15. `Re-Select Your VPC's (left hand menu) then the Flow logs tab.` You should now see a new flow log!

16. `Click the link for the Destination Name` (your s3 bucket). This should open up your S3 bucket in a new tab.

17. `Navigate the S3 folders for AWSLogs.` You should now (or very soon) notice a new folder in the Apache Hive format. eg aws-account-id={account-id}. If you select into the folder you will note we have NO VPC Flow logs so far.

So let's create an EC2 instance now and make sure it's in that VPC, so we can generate some VPC Flow logs!

>[!NOTE]
>If you are part of an instructor led lab we will cover off some more details about Apache Hive format which uses key=value pair folder structure. This method is generally the easiest way to optimise your searches. More on that later. AWS has some good documentation on this **[here.](https://docs.aws.amazon.com/athena/latest/ug/partitions.html)**


18. From your AWS console, `search for and select ec2.`

![Select EC2 Service from the AWS console ](/static/lab3/3b/select-ec2-service.png)

19. You will note there are no instances currently in your environment. `Click the Launch instance button` in the middle left-hand side of the screen.

>[!NOTE]
>This next section is broken down into multiple steps for creating an ec2. Pay close attention to the steps and screenshots.


## Creating an EC2 Instance ##


1. EC2 basic details. Fill in the below. See screenshot with example.
- **Name:** Name your ec2. e.g. mr-ec2
- **Application and OS Images:** Leave as Amazon Linux.
- **Instance type:** Leave as t2.micro.

![Enter the EC2 Service Name and Machine Type into the fields](/static/lab3/3b/ec2-machne-type-and-name.png)

2. Time to create an EC2 key pair and select it. From the Key pair section. Do the following:
- Next to the Key pair name-drop down, `Click Create new key pair`
    - **Key Pair Name:** Give your keypair a name eg mr-key-pair
    - **Private Key file Format:** Select format you prefer eg Putty or OpenSSH. 
    - **Click Create key pair** when ready.

>[!NOTE]
>We most likely won't use the keypair as VPC Flow logs will generate without it but, select the correct format for you in case we want to login!


![Create EC2 Key Pair with RSA and .ppk](/static/lab3/3b/create-ec2-key-pair.png)

**Key pair cont...** Make sure your new keypair is now selected in the drop-down.

![Select the new EC2 Key Pair that you have just created](/static/lab3/3b/key-pair-selected.png)

>[!NOTE]
>When you create the key pair you should have seen this download to your computer. Make sure you pay attention to where it went, you may need it later!


3. You can leave all remaining defaults and `click the Launch Instance` button to the right hand side. 

![EC2 Summary of the settings you have just configured](/static/lab3/3b/launch-ec2-instance.png)

4. If all things went well you should see a green box saying, **Success**. You will see a **instance ID hyperlink in the green box. Click this now.**

![EC2 Instance Hyperlink is shown in a green banner](/static/lab3/3b/ec2-instance-hyperlink.png)


## Verifying VPC Flow Logs ##

>[!NOTE]
>It can take up to 10 minutes for the first set of VPC flow logs to arrive in an S3 bucket. Time to stretch your legs! Or if your like me, go back to the S3 tab you had opened before (if not from the steps above go back to our S3 folder where the VPC flow logs should be). Press the refresh button till you see some logs. It should look like the image below when done. 
>
>![VPC Flow Logs Folder In S3 under Object and vpcflowlogs/ folder](/static/lab3/3b/vpc-flow-logs-folder-in-s3.png)

1. Now we see the logs, let's look at the format of a VPC Flow Log. `Select the first flow log from the list using the tick box and click the download button.` See example below.

![Select the VPC from the AWS console and navigate to the where the files are located at the smallest folder](/static/lab3/3b/vpcflow-log-download.png)

>[!NOTE]
>As we have chosen the parquet format for this lab you will not be able to view this file as you would a normal text based file. Parquet is a compress binary columnar format. This means we will have to open it up in something that can help us read it. There are many ways to do this but we have chosen as easy one for you. 


3. Go to the following website here: **[parquetreader.com](https://parquetreader.com/)** and drag your parquet downloaded file into upload section to open it up. See example below

![A Text editor showing an example VPC Flow Log that has been created as part of the workshop setup](/static/lab3/3b/example-vpc-flow-log.png)

>[!NOTE]
>Have a look at the VPC Flow log format. It has headings explaining each column type with key items such as account ID, interface ID etc.


This wraps up the first section of Lesson Two. Keep that file VPC flow log file handy as we will look at it in a little more detail in the next section.

**Congratulations!** That ends the first section of lesson two; configuring and exploring VPC Flow Logs. You should now have deployed your first VPC Flow log and see it land in your S3 bucket. Let's now more onto section two; Amazon Athena and Data Definition Language (DDL).

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3b_using_athena_ddl_for_aws_glue/2-exploring-amazon-athena-ddl-glue.en.md)</a> to continue. 
