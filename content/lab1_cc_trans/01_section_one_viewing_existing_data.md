## Section One: Viewing Our Existing Data

Let's start off by inspecting the existing credit card transactions currently being ingested into the Splunk platform.

### Searching Credit Card Details in Splunk
1. Open Search & Reporting from your Splunk Web home page by selecting `Search & Reporting` from the left-hand navigation menu. See example below:

![image_tag](/static/lab1/select-search-and-reporting.png)

2. In the search bar, `set the time range picker to be 15 minutes, and enter the following SPL into the search bar.`

```spl
index="cc_trans_im"
 ```

Example result: 
![image_tag](/static/lab1/sample-cc-trans-unfiltered.png)


Spend a few minutes reviewing the returned events.

>[!NOTE]
> Notice that unmasked PII (such as full credit card account numbers and sensitive customer details) are visible in plain text!

<br>
Now we have shown the raw un-masked data coming into the infrastructure monitoring index it is time to start using our Ingest Processor to achieve out goal of splitting this data up and filtering out the PII. 

### Click  <a>[next](/content/lab1_cc_trans/02_section_two_creating_first_blank_pipeline.md)</a> to continue on with the lab 