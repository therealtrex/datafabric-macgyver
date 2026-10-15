## Section One: Creating DDSS Connection in Splunk for Amazon S3

In this section we are going to use Splunk to create a connection to Amazon S3 that our DDSS service can use as an archival solution for our indexes. 

### Creating a connection ###

First part of this lab is to check the data coming into Splunk is actually hitting the DDAS index. 

1. From your Splunk Cloud console. `Click the Splunk cloud logo` top left-hand side. 
2. In the left hand menu under app. Click the Manage link as shown below: 


![image_tag](/static/lab6/select-manage-apps.png)

From here we can see a list of all of our apps installed. Its a bit hard to find but there is one for configuring DDSS connections.

3. In the box which says "filter by app name" `copy and paste the following in` and `hit enter`:

```
Self Storage Locations
```

You should see an app appear like shown below:

![image_tag](/static/lab6/found-selfs-storage-app.png)

4. `Click the link for the self storage app (under name)` to launch it. 

5. `Select the Green button called New Self Storage Location` on the top right-hand side. 

A popup will appear for filling in details for a new Self Storage Location. 

6. Enter the following details below:
- **Title:** cc-trans-pii-s3
- **Description:** Connection to S3 bucket for our DDSS used for the credit card transactions PII data. 

Before we fill out the rest of the table we must create an Amazon S3 bucket. 

>[!IMPORTANT]
>When you go to the AWS console next leave the current Splunk tab open as we will need to copy and paste between the two tabs. 

7. Note down the section where it says what the S3 bucket has to start (prefix) with. See example below

![image_tag](/static/lab6/s3-bucket-prefix-ddss.png)

8. Go back to your `AWS console from earlier in the labs and search for and select Amazon S3.` See example below:

![image_tag](/static/lab6/search-and-select-s3.png)

9. From the Buckets screen click the orange Create Bucket 

10. `Enter the name for the bucket using the prefix required and your name.` eg fs2-s3-shw-acfa9c34a81-qqq39jwv22wv-trex

11. `Scroll to the very bottom and click Create bucket.`

12. Go back to you Splunk page and enter in your bucket name in the required section as shown below:

![image_tag](/static/lab6/bucket-name-filled-in-ddss-app.png)

13. Leave the AWS S3 bucket folder empty

>[!NOTE]
>As part of the process we Splunk will automatically create HIVE partitions folder based on date and sourcetype etc. If you wanted to further add a folder then you could here as well. 

14. `Click the green Generate button` to generate the AWS S3 bucket policy. See example below

![image_tag](/static/lab6/s3-policy-generated-self-storage.png)

15. `Select inside the box and copy the policy to clipboard.`

16. Back in your AWS console, `go back to your AWS S3 bucket tab.` 

17. `Select the Permissions tab` as shown below:

![image_tag](/static/lab6/selecting-s3-permissions-tab.png)

18. `In the bucket policy box, click the blue edit button on the right-hand side.`

19. `Paste the bucket policy you copied from Splunk into the editor.`See example below

>[!NOTE]
>Reminder, all AWS account IDs are blanked out so your screen will look different. 

![image_tag](/static/lab6/pasted-aws-s3-bucket-policy.png)

20. `Click save changes when done.` 

21. Back on our Splunk tab again. Down the bottom, `click the green Test button.`

This will test that Splunk can write a file into AWS S3. We will show you that soon. See below for an example of successful test. 

![image_tag](/static/lab6/test-successful-s3-connection-ddss.png)

22. Lastly, back in our Amazon S3 tab again. `Scroll up and click Objects tab.`
23. `Hit the refresh button` (circle thing as shown below) to see our test file in the bucket. 

![image_tag](/static/lab6/test-file-lands-in-s3-ddss-setup.png)

24. `Back in the Splunk screen you can now click the submit button.`

That's it for this Section One. For our last section we will add the DDSS connection to our PII index for archiving. 

### Click <a>[next](/content/lab6_configuring_fs_ddss/02_configure_ddss_index.md)</a> to continue on with lab. 





