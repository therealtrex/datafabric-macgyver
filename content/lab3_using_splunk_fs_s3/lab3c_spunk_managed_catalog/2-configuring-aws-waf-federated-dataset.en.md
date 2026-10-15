For the final time now we are going to configure a Splunk Federated Dataset. This time for our newly configured AWS WAF Web ACL Logs. Hopefully by now the process should be getting familiar to you. 

## Setting up AWS WAF Federated Dataset in Splunk ##
Like before this will be a combination of jumping back and forth between Splunk and AWS. 

1. `Login to your Splunk instance if it has expired` and navigate to the Data Management app located on the left hand menu. See example below:

![Select Data Management App Splunk Cloud](/static/lab3/3c/selecting-data-management-app-in-splunk.png)

2. From here we should be taken into the Data Management App. `Click datasets` on the left hand side. 

3. `Click Create Dataset` on the right hand side to create a new dataset. 

4. Like before we need to `Select Amazon S3 and Click Next`

5. `Select the existing connection` from the drop down. `Click Next` when ready. 

6. Copy and paste below and enter into the Dataset Name field

```
aws_waf_logs
```

We now need to get our AWS WAF Logs S3 location. 

7. From your AWS console `go into S3 again.`

8. `Select the AWS WAF Logs S3 bucket` from the list. It should be named aws-waf-logs-splunk-{account id}-{region}

10. `Select into the folders until you get to the splunk-fs-s3-workshop-webacl folder.` 

11. `Tick the box next to the folder for the web acl` and `select the blue Copy S3 URI button`. See example below: 

![Select AWS WAF S3 URI](/static/lab3/3c/copy-aws-waf-s3-url.png)

12. Back on our Splunk create dataset page. `Paste in the AWS WAF URI we copied.` 

13. `Select the Federated Search radio button.`

See example below for filled out fields. 

![Page one AWS WAF Create Dataset screen](/static/lab3/3c/filled-out-aws-waf-create-dataset-page1.png)

14. `Click Next` when ready. 

15. This time we will `select I don't have a catalog`

16. A dropdown will appear asking which format the data is in. `Select JSON` and `Gzip for compress type`, see example below:

![Screen two no catalog setting](/static/lab3/3c/screen2-configure-dataset-nocatalog.png)

This will open up a lot more options for you to now fill in. 

>[!NOTE]
>Please take the time to now read the section on the Splunk page titled **Maintain catalog and dataset consistency**. This mentions important information on what we are doing next in the lesson. TLDR is we are going to configure an SQS queue for our AWS WAF S3 bucket to tell Splunk when to trigger a catalog update. More information on this process can be found **[here.](https://help.splunk.com/en/splunk-cloud-platform/search/federated-search/10.4.2604/run-federated-searches-over-amazon-s3-datasets/define-an-amazon-s3-dataset/create-an-amazon-s3-dataset-for-federated-search-that-is-backed-by-a-splunk-native-data-catalog/set-up-automated-updates-for-splunk-native-data-catalogs-in-aws)**


## Configure SQS and S3 Notification in Amazon ##
As described above, in this part of the lesson we will configure an AWS SQS queue and S3 notification to tell Splunk when we would like to update the Splunk managed catalog. 

1. From the AWS console. `Search for and select SQS`

![Select SQS](/static/lab3/3c/select-sqs.png)

You should see there is already one SQS queue created for us as part of the lesson. Eg splunk-fs-s3-{account-id}

2. From the list of SQS queue `click on the splunk-fs-s3-{account-id} queue.` See example below: 

![Select SQS Queue](/static/lab3/3c/select-sqs-queue.png)

3. `Click the copy blue button next to SQS ARN and paste this somewhere to use later on.` See example below:

![Copy SQS Queue ARN](/static/lab3/3c/copy-sqs-arn.png)

4. `In the Access policy box. Click Edit.` See example below: 

5. In the Access policy box, paste the following SQS access policy. 

```SQS-Access-Policy
{
  "Version": "2012-10-17",
  "Id": "example-ID",
  "Statement": [
    {
      "Sid": "example-statement-ID",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "YOUR_SQS_QUEUE_ARN",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:::YOUR_S3_BUCKET_NAME"
        }
      }
    }
  ]
}
```
6. `Replace the section YOUR_SQS_QUEUE_ARN with the one copied earlier`

7. Where is says **YOUR_S3_BUCKET_NAME**, `enter in your AWS WAF bucket name.` Eg aws-waf-logs-splunk-{account-id}-{region}

See example below of filled in sections.

![SQS Access Policy Done](/static/lab3/3c/sqs-access-policy-details.png)

7. Scroll down to bottom leaving everything else as is and `click Save`

You should see a saved configuration green bar on the screen. See example below:

![SQS Queue Access Policy Successfully updated](/static/lab3/3c/access-policy-updated-successfully.png)

Now this is updated we need to now tell S3 to trigger a SQS notification when the AWS WAF logs bucket is updated. 

## Create S3 Notification ##

1. From your AWS console, `search for and select Amazon S3.`

2. `Select your AWS WAF Logs bucket from the list.` Eg aws-waf-logs-splunk-{account-id}-{region}

3. `Select the Properties Tab.` See example below:

![Select properties tab S3 AWS WAF](/static/lab3/3c/select-properties-tab-s3-awswaf.png)

4. `Scroll to the section which says Event notifications`

5. `Click the button which says Create event notification`. See example below:

![Select Create Event notification on S3 properties](/static/lab3/3c/select-create-event-notification.png)

6. Copy and paste below into the Event Name field: 

```
aws-waf-s3-notification
```

7. For the prefix `copy and paste the path below updating with your AWS Account ID.`

```S3Path
AWSLogs/{account-id}/WAFLogs/cloudfront/splunk-fs-s3-workshop-webacl/
```

See example filled in below (note account id is blanked out below!): 

![S3 notification settings filled in one](/static/lab3/3c/s3-notification-setting-filled-in-one.png)

8. For event types `select the following options:`
- All object create events
- All object removal events
- All restore object events
- Lifecycle transition events
- All lifecycle expiration events

See examples below: 

![S3 notification event types 1](/static/lab3/3c/s3-notification-event-types-1.png)

and

![S3 notification event types 2](/static/lab3/3c/s3-notification-event-types-2.png)

9. `Scroll down to Destination section and select SQS queue radio button.`

10. `Select the SQS queue box and you should see our SQS queue in the list. Select this.` See example below: 

![Select SQS queue in S3 notification](/static/lab3/3c/select-sqs-queue-s3-notification.png)

11. Leave other defaults and `Click Save Changes` when ready. 

You should see a green successfully created event notification box. See example below:

![Successful S3 notification box](/static/lab3/3c/successfully-created-s3-notification.png)


## Continue on with Splunk Dataset Creation ##

1. Back in our Splunk dataset creation screen in the section for Do you want to keep catalog in sync with dataset. `Paste in the SQS queue ARN we copied earlier.` See example below: 

![Paste in SQS ARN Splunk Dataset AWS WAF](/static/lab3/3c/paste-in-sqs-splunk-dataset-aws-waf.png)

2. In the define schema section `select the option for Discover schema via crawler`

Leave settings for number of files to scan

3. In the time settings set the following for the fields. See example below: 
- Time field: **timestamp**
- Time format: **%UT%3Q**
- Unix time field: **_time**

![Settings filled in Splunk dataset aws waf 1](/static/lab3/3c/create-splunk-dataset-aws-waf-time-settings-discover-via-crawler.png)

4. Where it says Define partitions, are your partitions hive compatible, `select the option for No`

5. Enter in the following partitions settings into the box. See example below: 
- year: string
- month: string
- day: string
- hour: string
- min: string

![Enter in partition info AWS WAF Splunk Dataset](/static/lab3/3c/enter-in-partitions-splunk-dataset-aws-waf.png)

6. Leave remaining defaults and `click Next when ready.` 

7. `Select no for KMS keys` option. 

17. `Click generate policies.` Please review the policies and make sure they make sense to you before moving on. 

>[!NOTE]
>You should have noticed updated policies for the AWS WAF logs in the SQS settings and S3 bucket location. Normally these would need to be appended to the existing policies we used for Lesson One but the lab environment has used partial matches for its policies so we do not need to update them.  


18. `Click Next` when ready

19. `Click confirm` to acknowledge policy warning 

20. Review items and `click Create Dataset` when ready.

>[!NOTE]
>A blue box should appear notifying us that the new dataset has been created and is ready to be searched. So let's go try that. Note, the new dataset may take a few minutes to be actually ready. But don't worry we point that out if you do try and search to quickly. 


## Testing out new AWS WAF Log Federated Dataset ##

Its time now to run a Splunk search against our new VPC Flow log Federated Dataset. 
1. From the Splunk console. `Click the 9 set of square dots on the top right hand side, followed by the Search & Reporting link`. See example below 

![Select search and reporting in Splunk](/static/lab3/3c/select-search-and-reporting.png)

2. From the search bar, `select the SPL drop down and choose the SPL2 option` as shown in picture below:

![Select SPL2 from Search bar](/static/lab3/3c/select-spl2.png)

3. Using the code below, `copy and paste into the search bar.` You may need to adjust the time settings to something larger. e.g. All time. 

>[!WARNING]
>You will probably see an error saying Error in 'FasQueryProcessor': Dataset is still being created: Schema inference status: PROCESSING.
>
>Please wait about 5 minutes before searching. 


```SPL2
index=aws_waf_logs | head 100
```



4. Once completed you should see a result similar to the below. 
![Successful Splunk WAS WAF Logs returned in the Splunk search console](/static/lab3/3c/aws-waf-logs-search-returned-splunk.png)


**Congratulations!** on configuring AWS WAF logs to work with Splunk Federated Search for Amazon S3. You have picked up a few new skills in this lab. 
- You have learnt how to set up AWS WAF logs.
- You learn how to configure S3 notifications to SQS including their access policies.
- Lastly, you have now configured a Splunk managed catalog. 

That is the end of this double-click into Federated Search for Amazon S3. 

### Click <a>[next](/content/lab4_machine_data_lake_101/index.en.md)</a> to move into the next lab! 
