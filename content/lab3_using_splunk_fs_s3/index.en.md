# Cisco Data Fabric MacGyver Lab3: Splunk's Federated Search for Amazon S3 Service

## Scenario Overview ##
One of the goals of the `imnotbiased.com` team is to be able to access large volumes of data periodically for random use cases. Currently all of their large volumes of data reside in Amazon S3. For this use case we will dive into the Splunk service called Federated Search for Amazon S3. Over the next 3 parts of this lab we will take you through the understanding and basics of how Federated Search for Amazon S3 works in using different supported methods and capabilities. 

---

### Solution Strategy ##
To resolve this issue, we will use **Federated Search for Amazon S3** to search data from 3 sources of imnobiased.com these are:

* **CloudFront Access Logs:** Web traffic for their website
* **VPC Flow Logs:** Data for their network traffic inside their AWS tenancy
* **AWS WAF Logs:** Data from their internal AWS firewall logs. 

---

>[!IMPORTANT]
>These labs have come from part of a larger workshop for becoming a Federated MacGyver. This is a dedicated 8 hour course on Federated Search for Amazon S3. 
>
>Because of this there will be circumstances where things don't quite make sense, such as naming conventions that don't match exactly what we are doing within the labs for this workshop or screenshots which don't look right.
>I have done my best to line up everything as best I can but your instructor will be there to help you on the day. 

<br>

### Click  <a>[next](/content/lab3_using_splunk_fs_s3/lab3a_basics_of_fs_s3/index.en.md)</a> to start the lab. 