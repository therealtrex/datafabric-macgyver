## Section Two: Creating Our First Blank Ingest Processor Pipeline

### Log In to Splunk Cloud Services (SCS) ###

1. `Log in to your SCS tenancy` using the link provided in your Splunk SHOW lab environment.

>[!NOTE]   
> The portal should automatically authenticate using your cached credentials from your Splunk Cloud session. If authentication fails, ensure you are still logged into the Splunk Cloud instance used in Section One.


Once logged in, you will see the unified data management overview. 

1. Let's do a quick check of our destinations. `Select destinations on the left-hand menu.` See exmaple below

![image_tag](/static/lab1/datamanagement-select-destinations.png)

You should see the splunk_indexer already exists. This destination is how we will send data through ingest processor into Splunk Cloud for indexing. See example below:

![image_tag](/static/lab1/destinations_available_datamanagement.png)

Now that we can see the splunk indexer already exists we can move onto our first pipeline. 

>[!NOTE]   
>You will see the section for Edge Processors. This is where you would go to manage your edge processors if you are using them. Splunk data management allows you to manage edge and ingest processors as well as pipelines all in the one single management interface! 

2. Select `Pipelines` from the left navigation menu.

![image_tag](/static/lab1/datamanagement-select-pipelines.png)

3. Click the blue `+ New pipeline` button in the top right of the screen.

![image_tag](/static/lab1/select-new-pipeline-ingest-processor.png)

Welcome to the ingest processor pipeline wizard. From here you will see the major steps at the top and then on the right you can see a getting started section. 
Pause, take some time to have a look at all of the different templates out of the box we offer for different data sources. This is a great way to get started and save you lots of time. 


5. When ready, select `Blank pipeline`, then click `Next.`

![image_tag](/static/lab1/pipeline-select-blank-pipeline-next.png)

From here we will see there is four ways we can partition (or intercept) our data before its indexed in Splunk. These are **sourcetype, source, host and index** (you may need to scroll to see this). 

**For this lab we are going to partition by index.**

>[!IMPORTANT]  
>As you can imagine. What you select depends on your data. If we had selected sourcetype and we used a very common sourcetype then it would be more difficult to manage the data we care about or worse. We could manipulate the wrong data!
>Seeing as all of our credit card data is going into the one index and no other data is also going into that index, we are ok to use index as partition type.

>[NOTE]
>One thing to note as well in this TLDR section. The index value here is not data from an index, which might be your first thought. We are saying if data coming in has a metadata value set of index=xyz then we can match on that result. 

6. Select the `index` partition option.

![image_tag](/static/lab1/partition-by-sourcetype.png)

7. In the **Value** field, copy and paste the following:

```text
cc_trans_im
```
>[NOTE]
>Before we click apply you will note we can use operators as well to do partition matches, eg is NULL or my favourite, is not NULL. These can help us to be more flexible in the value to match against in our index field. 

8. `Click Apply when ready.`

From here you can see we have now set a partition in the wizard. See example below:

![image_tag](/static/lab1/pipeline-partition-set-to-index.png)

9.	When ready `click Next`

From here the Ingest processor would like us to provide it some sample data to use for our pipeline. We have two options. We can either upload a sample of the data which will be used, or we can actually take a live sample from our real data set. 

Let’s configure a live sample set. 

10. `Select Capture new snapshot` from the options. 

11. From here `enter in cc_transactions where it says Name`, we will leave the rest of the defaults for maximum interval and events. See example below:

![image_tag](/static/lab1/capture_cc_transactions_sample_ingest_processor.png)

12. `Click capture` when ready. 
13. We should now see our capture selected. `Click Next` for our final step in the setup wizard. 

The final step of the wizard is to select our destination. We already noticed earlier that we have splunk_indexer already configured as a destination. 

14. Go ahead and `select splunk_indexer` from our list. 

From here we will see three options. Read below for an explanation of each:

**Default:** Use the index field in the metadata (eg cc_trans_im). Otherwise it will use a default one set for the deployment.

**Specify index for events with no index:** This is used for circumstances when there is no metadata index is set as the data is coming in. When none is set then use the index you select here. Eg if metadata index is set to t-rex then the index is t-rex otherwise it will be set to what we say here. 

**Specify index for all events:** This is the just overwrite everything setting and just send to an index we choose. 

Which is the right answer for us? Pause for thinking time...

The most correct answer is **Specify index for all events.** This is because we are matching against an index field. If we were using host or sourcetype then there are potential different outcomes here. Because we are doing an exact match against an index. We are always having the index match. 

>[NOTE]
>All of this may not matter anyway if you explicitly set your index fields during a pipeline. Will make sense later. 

15. When ready `select Specify index for all events` and `enter in cc_trans_im` as the value. This is where we want to send the data after it is redacted. See example below: 

![image_tag](/static/lab1/index-selected-destination-pipeline.png)

16. `Click Done` when ready. 

### Pipeline editor ###

Welcome to the pipeline editor. There are three main sections on this screen:
**Top:** Is your pipeline code
**Bottom:** Is the preview of your post pipeline
**Right:** Is the pipeline overview section showing you the setup of your pipeline. 

See example below:

![image_tag](/static/lab1/pipelineoverview-cctrans.png)

If you are doing this lab as part of an instructor led course then your instructor will explain this to you in more details on how ingest processor pipelines are made up. 
Basically this pipeline is just taking our source (cc_trans_im index) data, setting the index of cc_trans_im again and piping it into our destination of Splunk. 

   $pipeline = | from $source
       | eval index = "cc_trans_im" | into $destination;

Lastly, in the preview section you can see the event broken out into columns of **_time, _raw, host, index, sourcetype and source.** 

Now it is time to start building out our requirements for the lab which were:
- Send the raw logs to a new special PII index called cc_trains_pii
- Filter and mask the original raw log 
- Put that filtered and masked data back into the cc_trans_im index for the infrastructure monitoring team. 

### Click <a>[next](/content/lab1_cc_trans/03_section_three_build_out_our_credit_card_pipeline.md)</a> to continue on with the lab 
