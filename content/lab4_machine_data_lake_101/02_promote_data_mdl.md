## Section Two: Routing Data to Machine Data Lake from Ingest Processor

Now we have a new MDL dataset, let's route our linux_secure data to it.

### Ingest Processor Machine Data Lake Destination ###

1. From our Splunk SCS tenant `select the Splunk logo`, top left-hand side. 
2. `Select the pipelines` option left-hand side.

We will see our two applied pipelines here for cisco_asa and cc_trans data. See example below:

![image_tag](/static/lab4/current-two-pipelines.png)

3. From here `click +New Pipeline, followed by Ingest Processor pipeline`

4. 