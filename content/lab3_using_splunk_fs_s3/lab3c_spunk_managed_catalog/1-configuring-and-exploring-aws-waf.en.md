For this first part of our Lab we will explore how to configure AWS WAF logs to send to Amazon S3. We will then explore the format of the logs in preparation for the next part of our lab in setting up another Amazon Glue table. 

## Configuring and Exploring AWS WAF Logs ##
1. Let start by first going to WAF. From our AWS console. `Search for WAF` and `select WAF & Shield`

![Select WAF & Shield in the AWS console](/static/lab3/3c/select-waf&shield.png)

>[!WARNING]
>If you don't see the same screens as below. Then this could be the workshop deploying using the older AWS WAF console experience. To fix this on the left hand side right down the bottom there should be a link which says **Switch to the new WAF console**. Click this button and the rest of this lab should now look right! See example image below
>
>![Switch to new WAF Console](/static/lab3/3c/switch-new-waf-console.png)


This will take you to the Resources and protection packs (web ACLs) page. Note the currently configured web acl for splunk-fs-s3-workshop-webacl and that it has no logging enabled.

![AWS WAF Web ACL Screen](/static/lab3/3c/aws-waf-web-acl-nologging.png)

2. `Select the splunk-fs-s3-workshop-webacl` Web ACL from the list. A pop up should appear on the right. 

3. `Select the last option` for `Configure Logging and samples requests.` See example below:

![Example pop up right hand side web acl](/static/lab3/3c/show-web-acl-properties.png)

4. From here you are taken to the Configure logging section. From the drop down on the right hand side `Select Enable, Logging destination.` See example below. 

![Select Logging Destination AWS WAF](/static/lab3/3c/aws-waf-select-logging-destination.png)

5. A new popup will appear on the right hand side. `Select S3 from the drop down.` See example below

![Configure AWS WAF Logging S3 Selected](/static/lab3/3c/configure-waf-logging-s3-selected.png)

>[!NOTE]
>**AWS WAFv2 requires WAF logging buckets to start with the prefix `aws-waf-logs-`.** Good news: the workshop's CloudFormation stack has already pre-created a bucket named `aws-waf-logs-splunk-{account-id}-{region}` for you, so there's nothing to create here.


6. `Click inside the Enter S3 bucket name box` and select the aws-waf-logs-splunk-{account-id}-{region} s3 bucket. See example below:

![Select AWS WAF S3 log bucket](/static/lab3/3c/select-s3-aws-waf-logging.png)

7. `Leave all other defaults` and `click Save` at the bottom of the page.

You should now see your Logging section showing a logging destination of S3 pointing at `aws-waf-logs-splunk-{account-id}-{region}`.

## Exploring AWS WAF Logs ##

1. Let's explore those new logs. From the Web ACL login screen you were just on, `click the link to the AWS S3 Bucket.`

![Open AWS WAF Logs S3 Bucket in the AWS console](/static/lab3/3c/open-s3-bucket-aws-waf-logs.png)

>[!NOTE]
>It may take a few minutes for logs to start appearing. 


2. `Click the folders for your new S3 bucket until you see your first set of logs.` 
- e.g. AWSLogs/{Account ID}/WAFLogs/cloudfront/splunk-fs-s3-workshop-webacl/{year}/{month}/{day}/{hour}/{minute}. See example below.

![Folder Structure AWS WAF Web ACL Logs is displayed at the top of the screen](/static/lab3/3c/folder-structure-aws-waf-logs.png)

>[!NOTE]
>You will note AWS WAF Web ACL logs use the following format by default. AWSLogs/account-id/WAFLogs/Region/web-acl-name/YYYY/MM/dd/HH/mm
>
>That's quite a few folders and lots of complexity to think about when configuring a catalog for FS-S3 searches.


3. Let's have a look at a file. `Click the tick box for the first file and select the download button`. See example below. 

![Download WAF Log with the download button on the right-hand side of the page](/static/lab3/3c/download-waf-log.png)

4. `Open the file you just downloaded to have a look at the format.` Below is an example of the WAF Web ACL File. 

![Text showing Sample AWS WAF Web ACL Log](/static/lab3/3c/sample-aws-waf-web-acl-log.png)

>[!NOTE]
>**AWS WAF Web ACL Log Format:** You will see in the example the log file is in a structured JSON format. This is Different to anything else so far in this workshop. AWS WAF logs can also contain lots of different types of JSON artifacts / information depending on what's being validated and the type of information it is displaying.  


Now that we have configured our AWS WAF Web ACL logs to go to Amazon S3 it is now time to try out the Splunk managed catalog option. Click next when ready. 


**Congratulations!** You have successfully explored AWS WAF log data for more details on blocked requests.

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3c_spunk_managed_catalog/2-configuring-aws-waf-federated-dataset.en.md)</a> to begin! 