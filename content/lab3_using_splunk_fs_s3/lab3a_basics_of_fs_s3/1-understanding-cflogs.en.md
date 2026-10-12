To kick things off, let's explore our CloudFront distribution which was spun up as part of the workshop.

## Explore CloudFront Log Setup ##

1. From your AWS console search for and **select CloudFront**

![Select CloudFront from the AWS console](/static/lab3/3a/select-cloudfront.png)

2. Welcome to the CloudFront service. From here by default you should see the Distributions section of CloudFront.  If not select **Distributions** from the left-hand menu.
3. You should see at least one distribution deployed as part of the workshop. You will note one has a description of **Sample Web Application.** Enter this distribution by **selecting the hyperlink ID** for it. See example image below.

![Select Sample Web Application Distribution](/static/lab3/3a/select-distribution.png)

4. Feel free to explore the distribution configured as part of the workshop. When Ready **select the Logging tab.**

![Select logging tab on the top of the page](/static/lab3/3a/distribution-logging-tab.png)

5. From here you can see there is currently no standard log destinations configured. Let's create one by **clicking the Add** dropdown followed by **Amazon S3** as shown below

![select Add and from the dropdown Select Amazon S3 CloudFront Logging](/static/lab3/3a/select-amazon-s3-cloudfront-logging-setup.png)

6. In the Standard log destinations section, **click the text which says Select a S3 bucket**, this will show a drop down of all S3 buckets. 

7. From the dropdown **select the bucket with the name of splunk-fs-s3-logs-{account-id}-{region}**.

8. Next **Click the arrow for Additional settings - optional.**

>[!NOTE]
>We will be leaving the remaining defaults, but you can see from the options below we have the ability to select different fields which will modify the log output format. We can also add folders for partitioning. This can be useful if you are wanting to search logs from different distributions or dates etc. Feel free to review these (without changing) before moving on.

9. Leave all the remaining defaults and **click Submit.** See example below.

![Fill out CloudFront S3 Logging Details in the options presented](/static/lab3/3a/fill-out-s3-details-cloudfront-logs.png)

>[!NOTE]
>Now the logging is enabled, feel free to have a read below some additional information on the CloudFront Access Logs S3 type shown in the logging tab. 
>- **Destination:** AWS Service where the logs are being shipped too. This can vary depending on selected type. For S3 it is the chosen S3 bucket.
>- **Type:** Specifies the logging type selected (e.g. CloudWatch, Firehose or S3). You will note we selected S3. 
>- **Selected Fields:** Enables you to select which fields to include and in what order for each log record. We have left default (all fields, approx 33). 
>- **Partitioning:** Is the folder location where the logs will be delivered too. This has been set to AWSLogs/{account-id}/CloudFront. 
>- **Output format:** This is critical to know. For CloudFront there are 5 options to choose from JSON, Plain, w3c (default), Raw and Parquet. Choosing differently will affect your Glue Table configuration as format differs.


## Explore CloudFront Logs in S3 ##

1. You should now see a Standard log destination configured for our CloudFront Distribution. **Click on the hyperlink for the destination** as shown in the picture below. 

![Open up S3 from Logging Tab as you did before](/static/lab3/3a/selectS3fromcloudfrontlogging.png)

>[!NOTE]
>As we have just turned on logs it can take up to around 3-5 minutes for logs to start appearing in your S3 bucket. 

2. From the S3 bucket you should see an initial folder of **AWSLogs/**. Navigate all the way into the CloudFront folder by **selecting each of the folders for AWSLogs, {account-id} and then CloudFront**. You should see logs files as shown in the image below.

![View CloudFront Access Logs in S3 bucket section of the console](/static/lab3/3a/viewcloudfrontlogsins3.png)

**Congratulations!** In this part of the lesson we explored CloudFront access logs and saw them being created in S3. Next we will take a look at our first AWS Glue table.

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3a_basics_of_fs_s3/2-understanding-glue-catalog.en.md)</a> to begin! 

