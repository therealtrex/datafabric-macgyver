## Section Three: Build out our credit card transactions pipeline

It takes a while to learn how to build pipelines from scratch. But thankfully now days there are plenty of tools and examples to help. For this lab we will provide you everything you need but in future you can use examples we showed as part of the pipeline building or you can even use AI to generate a good starting point. 
Let’s start by splitting our data into two different indexes. 


### Building our first pipeline ###

1. `Copy and paste the following code block **totally replacing** the existing code`

```
$pipeline =
   | from $source
   | branch 
   [
    | eval index = "cc_trans_im"
   ],
   [
    | eval index = "cc_trans_pii"
   ]
   | into $destination;

```

>[!NOTE]
>This pipeline will now **branch** the data into two streams. This function takes sections enclosed in hard brackets. Inside each bracket we are setting the output index value where splunk will route the data. 
>At this stage if we applied this pipeline all it would do it create a direct copy of the raw incoming data and send it to both indexes. 
>More info on the branch command can be found <a>[here](https://help.splunk.com/en/data-management/process-data-at-ingest-time/use-ingest-processor/route-data-using-pipelines/process-multiple-copies-of-data-using-ingest-processor)</a>

Now we have the basics down we can create a re-usable routine to start to strip out PII data. 

2. `Copy and replace the following code, again completely replacing your existing one`

```
$pipeline =
   | from $source
   | branch 
   [
    | drop_non_storable
    | eval index = "cc_trans_im"
   ],
   [
    | eval index = "cc_trans_pii"
   ]
   | into $destination;

function drop_non_storable($source) {
   return
   | from $source
       // Remove Track 1 Regex
       | eval _raw = replace(_raw, /,"TRACK1":"%B[0-9]{0,19}\^[A-Za-z0-9_ ]{2,26}\^[0-9]{7}[A-Za-z0-9_]*\?",/i,"")
       // Remove Track 2 Regex
       | eval _raw = replace(_raw, /"TRACK2":"[0-9]{13,19}=[0-9]{7}\?"/i,"")
       // Remove PIN BLOCK Regex
       | eval _raw = replace(_raw, /"PIN_BLOCK":"[0-9A-F]{16}",/i,"")
       // Remove CCV Regex
       | eval _raw = replace(_raw, /"CC_CCV":"[0-9]{3,4}",/i,"")
       // Remove PIN Regex
       | eval _raw = replace(_raw, /"PIN":"[0-9]{4}",/i,"")
}
```

>[!NOTE]
>The new pipeline adds a sub-routine called drop_non_storable. We don\’t expect you to understand exactly what it\’s doing but basically as the text explains there are five regex blocks which run through the _raw event to drop PII data with text like TRACK1, TRACK2, PIN_BLOCK, CC_CCV and PIN. >That way the infrastructure monitoring team cannot see this data. 

Now is a good time to look at what\’s changed and show you the preview function. 

3. From the top right-hand corner `press the blue play button.` See example below:

![image_tag](/static/lab1/preview_cc_trans_pipeline_redacting_data.png)

You will see when pressing the play button the preview section down the bottom changes, showing us what we implementing. 

Below is a before and after so you can see easily what that code did. 

**BEFORE (see highlighted sections)**

![image_tag](/static/lab1/cc_trans_before_data_removed_highlighted.png)

**AFTER (see highlighted sections from before are gone!)**

![image_tag](/static/lab1/cc_trans_after_data_removed.png)

>[!NOTE]
>At any point you can go back and forth using **//** in front of the drop_non_storable function and pressing the preview button again. This will magically bring the original data back. 
>The good news is that if the business decides that nobody can have that data you can put it into both sections of the branch to remove it from both feeds. 

Now let\’s add another function to mask critical data from the infrastructure monitoring team index. 



