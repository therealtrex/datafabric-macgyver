Now we have been through the basics of CloudFront Access logs and Amazon Glue tables it is now time to see how these come together with Splunk to build out our first Federated Search!

## Creating a Splunk Amazon S3 Connection ##

>[!NOTE]
>For this part of your lab you will need your Splunk Instance provided as part of your workshop or an instance of Splunk which you are using that has Federated Search for Amazon S3 feature enabled.


1. First we need to go to the **Data Management** app in Splunk. **Select the Data Management** app located on the left hand menu in Splunk. See example image below.

![Selecting Data Management App](/static/images/lesson1/selecting-data-management-app-in-splunk.png)

2. You should now be on the Data Management page. There is a lot to take in here but for now let's just dive straight in. **Click Connections** on the left hand menu. See example below 

![Select Data Management Connections](/static/images/lesson1/select-data-management-connections.png)

3. From here we will need to create a new connection. **Select the blue Create connection** button located on the right hand side. See example below. 

![Select Create Connections Button](/static/images/lesson1/select-create-connection-button.png)

4. From here we need to select **Amazon S3** radio button followed by **Next**

![Select Amazon S3 Provider Radio button and press next](/static/images/lesson1/select-federated-s3-provider-radio.png)

>[!NOTE]
>You should now see the Create connection's section. From here we will be swapping between AWS Console and Splunk to fill in certain fields to create the connection.


5. In the **General** section, enter a **Connection name** e.g. fs-s3-workshop. Optionally also add a description.
6. Next, **enter the AWS account ID** associated with your lab. This can be found from your top right drop down in your AWS console See example below.

![An example of the AWS console that shows the Account ID and Federated user details ](/static/images/lesson1/aws-account-id-example.png)

>[!WARNING]
The AWS Account ID has a **copy** button. This should be used to copy the account ID as it removes the '-' from the account ID displayed in sample image above. If you copy directly using your cursor you should remove the '-' from the text.


>[!NOTE]
>In the Splunk console under the section for AWS region there is a note. This note is mentioning that it is recommended to use the same region as your Splunk Cloud platform to save on egress costs. 


7. **Select the region** where your workshop is running in. Eg us-east-1. 
8. **Click Next** to go onto the next screen. 

>[!NOTE]
>This next section allows us to define the abilities (or use cases) for our connection. The Splunk Data Management app can define connections for Federated Search, Ingest Processor and Edge Processor. For this workshop we will only be using Federated Search.


9. Select **Run Federated Search** tick box only. **Click Next** once done. See example below:

![Selecting FS S3 Abilities](/static/images/lesson1/selecting-abilities.png)

>[!WARNING]
>This next section is tricky so read carefully! 


>[!NOTE]
>The storage authentication screen allows us to copy a custom trust policy into our AWS workshop account. This allows the Splunk cloud to assume a role that we assign this trust policy too. Any polices (more on that later) that are assigned to that role are permissions Splunk is allowed to use. This can be a little tricky to understand so feel free to read more around trust policies **[here.](https://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles)** 


10. **Leaving the Splunk tab open**, we need to append this trust policy to a pre-created role we have in our AWS workshop account. From our AWS console **search for and select IAM**

![Select AWS IAM service](/static/images/lesson1/select-iam.png)

11. From your IAM screen **select Roles** on the left hand side. 
12. In the search section **enter Splunk** and you should see a filtered list of two pre-created roles as part of the workshop. See example below: 

![Filter on Splunk roles in IAM](/static/images/lesson1/filter-splunk-roles.png)

13. **Select the SplunkFs-S3role from the list** eg splunk-fs-s3-workshop-SplunkFsS3Role-xxx
11. From here, **select the Trust Relationships tab** where you should see a sample trust relationship already. 
12. **Click Edit trust policy**
13. **Paste in trust policy from Splunk replacing the old one** into the editor and then **click Update Policy** when ready. See example below:

![Past trust policy IAM](/static/images/lesson1/past-trust-policy-iam.png)

14. From here we need to copy the ARN of the role. **Click the blue boxes next to the ARN section.** See example below 

![Copy role ARN](/static/images/lesson1/copy-role-arn.png)

15. Return to the Splunk screen and **paste the role ARN into the section where it asks for it.** See example below:

![Paste role arn into Splunk](/static/images/lesson1/paste-role-arn-into-splunk.png)

>[!WARNING]
>You will note an important message (see example below) about adding a tag to the role. This is used as part of the validation the Splunk side.


![Adding storage tag on role](/static/images/lesson1/adding-splunk-tag-storage-authentication.png)

16. The last step on the storage authentication screen is to add the tag to the AWS IAM role. On your AWS screen where your role was being configured, **select the Tags tab.** See example below:

![Select Tags](/static/images/lesson1/select-tags.png)

17. You should now be able to **click the Add new tag** button in the middle of the screen. On the next screen **Click Add new tag**

18. **Check your splunk screen** for the correct tag name and value. **Enter this in your AWS Tag screen.** eg key is splunk-assumable-role and value of true. See example below: 

![Add Tag](/static/images/lesson1/added-tag-to-role.png)

19. Once confirmed **Click Save Changes** button to save your tag. 

>[!NOTE]
>Once saved the tag should be show in your list. 


20. Back in the Splunk console you can now **tick the box confirming you have added the tag** and **click Next** to continue. 

21. On the Catalog Authentication page **Select No** for Apache Iceberg REST options and **Click next** when ready. 

>[!NOTE]
>Apache Iceberg REST authentication is a type of catalog structure which we aren't using at this point in the lab. 


21. Review your settings and when happy **Click Create**

22. Once done, you should now see a blue **connection created** screen. You can now move onto creating your first dataset. 

![Created connection](/static/images/lesson1/connection-created.png)


## Creating CloudFront Dataset ##

Now we have created a connection, we need to create our first dataset for CloudFront Access logs. 


1. **Select Datasets** section on the left hand Splunk menu. See example below:

![Select datasets](/static/images/lesson1/select-create-datasets.png)

>[!NOTE]
>This is the section in the data management app where you create your different datasets for Federated Search, Edge Processor and Ingest Processor. 


2. **Click the blue Create dataset button.**
3. Like before, **Select Amazon S3** from the list. **Click Next**
4. **Select the connection we created just now.** Once selected, **Click Next**

![Select connection](/static/images/lesson1/select-connection.png)

5. **Name your dataset** eg cloudfront_access_logs

For the next step of Amazon S3 location we need to go back to the AWS console. 

6. From your AWS console, **search for and select Amazon S3.**

![Select S3](/static/images/lesson1/select_s3.png)

7. From here **select the logs bucket for our lab.** Should be splunk-fs-s3-logs-{account-id}-{region}

8. Using the links on the folder **select the folders till you see the CloudFront folder.** 

9. **Tick the box for CloudFront** and **Select Copy S3 URI.** See example below

![Copy S3 URI](/static/images/lesson1/copy-s3-uri-cloudfront.png)

10. **Paste the S3 URI back on the Splunk screen for Amazon S3 location.** 

11. **Select the Federated search radio button and Click Next**

12. Splunk asks you which type of catalog you use for managing your data. For this lab we will be using the AWS Glue method. **Select AWS Glue**

>[!NOTE]
>After selecting AWS Glue, a whole bunch of new fields appear. Most of the fields information can be obtained by looking at the AWS Glue catalog settings we have created for you in this lab. 


13. From your AWS console. Go back to the AWS Glue tab or re-open it and **select the splunk_cloudfront_access_logs_w3c table as you did before.**

14. From the Glue table, table overview. You can see most of our required settings. See them highlighted in the screenshot below

![Glue table summary](/static/images/lesson1/glue-table-summary.png)

15. On the Splunk screen enter the **AWS account id as the AWS Glue catalog ID.** This can be obtained top right hand side of AWS console. 

>[!NOTE]
>You can confirm this if you wanted to. On your AWS Glue console. **Select Catalogs from the left hand menu.** You will see here your catalog name is your AWS account id.


16. **Enter in the AWS Glue Database name from your Table overview** eg splunk-fs-s3-{accound-id}
17. **Enter in your AWS Glue table name from your Table overview.** eg splunk_cloudfront_access_logs_w3c
18. **Select Non-table format** as the format for our table. 

See example so far:

![Data set settings CloudFront One](/static/images/lesson1/configured-data-set-cloudfront-settings-1.png)

19. In the Time settings details enter the text of **time for Time field** , **%H:%M:%S for Time format** and **_time as Unit time field** as shown in the example below:


![CloudFront Federated Index Time Settings page](/static/images/lesson1/cloudfront-federated-index-time-settings.png)

>[!WARNING]
>Time settings in Federated Search can break or alter the outcome of your search result which could lead to changes in cost as well. CloudFront Access Logs uses a 24 hour **Hour Minute Second** format for time. This is why we have chosen %H:%M:%S in the time format settings for the federated >index. If you are attending this workshop as part of a joint Splunk / AWS immersion day we will explain in further details around the time settings and how they maybe important to your environment. More information can be found **[here.](https://help.splunk.com/en/splunk-cloud-platform/search/federated-search/10.4.2604/sdselect-command-legacy/use-time-fields-in-sdselect-searches)** 


20. Leave timezone settings as is and **select Next to continue.** 

21. We are not using KMS keys for this lab so **select No** for KMS keys on the next screen. You should now see a button appear for Generating Policies. 

22. **Click Generate policies** button. Please take the time to study these before clicking next. 

>[!NOTE]
>If you were doing this in your own environment you would need to create policies using the ones generated. These are then attached to the role we modified earlier that we assume permissions from. Thus giving Splunk the permissions needed to run the actions we need in your account. 
>
>This lab has these policies already defined so we wont need to do this. If you are running this in an instructor led course your instructor will explain these policies too you. 


23. **Click Next when ready**
24. Read the note about AWS policies and **click Confirm** when ready. 
25. Review the settings and **Click create Dataset**. See example below. 

![CloudFront Dataset summary](/static/images/lesson1/summary-create-cloudfront-dataset.png)

You have now created your first dataset. It is now time to search it and see if it is working correctly. 

## Running our first Federated Search for Amazon S3 ##

Now it's time to test if all of our hard work has paid off. Let's try our first search against data in CloudFront Access Logs. 

1. From the Splunk console. **Click the 9 set of square dots on the top right hand side, followed by the Search & Reporting link**. See example below 

![Select search and reporting in Splunk](/static/images/lesson1/select-search-and-reporting.png)

2. If prompted, **Click Don't show me this again** for the victoria experience box. 

>[!NOTE]
>Federated search uses SPL2 as its primary query language. Please note that all searches run in this lab should be run using SPL2 search type. We will show you how to select this if not already.  


3. From the search bar, **select the SPL drop down and choose the SPL2 option** as shown in picture below:

![Select SPL2 from Search bar](/static/images/lesson1/select-spl2.png)

4. From the Splunk search window. **Adjust the Time range** to be **All time** and **Paste the search below into your search window and run the search.**

>[!WARNING]
>The first time you run this it may give you a warning saying **Resources are being provisioned for your search, Provisioning can take a few minutes.** If you see this please wait about 3-5 minutes and try your search again. 


```SPL2
index=cloudfront_access_logs  | head 100
```

>[!NOTE]
>There is also a few different ways in which you can search with federated searching using SPL2. We will look at that a bit later. Right now we are just trying to test the data can be successfully searched. 


![Successful First CloudFront Access Logs Search](/static/images/lesson1/successful-first-search-cloudfront.png)


**Congratulations!** You have now successfully set up your first Splunk Federated Search for Amazon S3 again CloudFront Access Logs. With that you have now completed the lesson! Well done!

### Click <a>[next](/content/lab3_using_splunk_fs_s3/lab3b_using_athena_ddl_for_aws_glue/index.en.md)</a> to begin.
