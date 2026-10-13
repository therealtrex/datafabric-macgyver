## Section Two: Promoting Data with Machine Data Lake

Now we have routed some data to Splunk's Machine Data Lake we are helping to optimise the team at imnotbiased.com data to match performance vs price. In this next section we are going to show you how you can easily promote a subset of data to DDAS for queries which need the extra performance and analytics capabilities. 

### Promoting Data in Machine Data Lake ###

>[!NOTE]
>Last time we showed you how to connect to the Splunk catalog service via the menu option. This time we will show you via the shortcut on the main Splunk Cloud screen. 

1. From our Splunk Cloud console. `Click the catalog icon` near the top-left hand side. See example below 

![image_tag](/static/lab4/catalog-shortcut-button.png)

2. Like before, `select our linux_secure_mdl catalog.` See example below:

![image_tag](/static/lab4/mdl-table-summary-catalog.png)

From here you will see our Promote button. This is used to promote Splunk catalog data into Splunk DDAS or an Analytics table which is part of machine data lake. You instructor will explain more around the Analytics table in your course. 

3. `Click the blue promote button.`

From here a promote wizard should open, see example below: 

![image_tag](/static/lab4/mdl-promote-wizard-overview.png)

From here we have three main options:
- **Static Splunk index:** This is used to promote data into DDAS (tsidx) format. 
- **Static analytics table:** This is used to promote ata into the Machine Data Lake Analytics Table 
- **Streaming Splunk index:** This is used to send a stream of data from MDL into Splunk DDAS as it arrives in MDL. 

More information on these can be found <a>[here](https://help.splunk.com/en/splunk-cloud-platform/land-data-in-machine-data-lake/1.0/promote-data)</a>

For this use case we want to do a one-shot static Splunk index promotion. 

4. `Leave Static Splunk index radio selected.`

In the Time range section for a bit of fun we will only bring in the last 5 minutes of data. 

5. `Click the box which says Last 1 day` and `adjust the time range picker to be 5 minutes ago.` See example below:

![image_tag](/static/lab4/select-5-minutes-ago-mdl-promote.png)

6. `Click apply`, to accept time.

7. `Click the + Add filter` button. 

From here like our pipelines we can filter MDL data based on key fields. Eg source, sourcetype and host. This can be handy when we only want to bring in certain type of data from our MDL raw table. 

8. `Leave sourcetype selected and enter in the text below.`

```
linux_secure
```

See example below for filled out configure page:

![image_tag](/static/lab4/filled-in-mdl-configure-page.png)

9. When ready, `click Next` to continue. 

The next page is the preview data page. Currently there should be no data in preview. See example below:

![image_tag](/static/lab4/preview-mdl-promotion-blank.png)

You will notice the timerange and sourcetype is set. 

10. `Click the blue play button` to preview our data which we will promote. See example below:

![image_tag](/static/lab4/mdl-promote-preview-linux-secure.png)

11. When ready, `click Next`

The final page asked us for a index (dataset name) and optional description. Replace the dataset name with the one below

```text
linux_secure_mdl_promoted_5mins
```

12. `Enter the description below` into the field. 

```text
Promoted last 5 minutes of the Linux_secure source for analysis 
```

See example below for filled in review page

![image_tag](/static/lab4/filled_in_mdl_preview_page.png)

13. When ready, `click the blue Start promotion button.`

After a few moments you should see a promotion kick off. it will start in pending status. See example below

![image_tag](/static/lab4/mdl-promotion-pending.png)

14. After a minute or two click the Refresh jobs button. 

You may see the next stage appear which is In progress. See example below

![image_tag](/static/lab4/mdl-promotion-in-progress.png)

Continue to push the refresh job button after a few more minutes and our promotion should change to complete! See example below

![image_tag](/static/lab4/mdl-promot-stats-completed.png)

Now it is completed. Let's see if it worked! 

15. `Click the datasets tab` near the top left-hand side. It should be next to the Promotion job tab currently highlighted. See example below. 

![image_tag](/static/lab4/select-datasets-tab-mdl.png)

On the top of our list we should see our new index/dataset for the linux_secure_mdl_promotedd_5mins. 

16. `Click our new dataset` to open up the details tab. 

17. `Click the blue search button` as shown below: 

![image_tag](/static/lab4/new_mdl_dataset_linux_click_search.png)

From here a new tab will open and automatically search our data. Before you even read this sentence the data will already have appeared as we are now using DDAS super fast storage! See example below:

![image_tag](/static/lab4/mdl-promotion-search-result-ddas.png)


That is it! Congratulations we just promoted some data from a Machine Datalake Raw table into Splunk DDAS index for searching. How easy was that! That is it for this lab. Its time to move onto the next lab. 

### Click <a>[next](/content/lab5_machine_data_lake_ingest/index.en.md)</a> to move onto next lab. 





