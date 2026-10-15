## Section One: Creating our second MDL table

In section one we are going to validate the data and then create our second MDL table which we we will use to route access_combined data into. 

### Validating the problem ###

First part of this lab is to check the data coming into Splunk is actually hitting the DDAS index. 

1. From your Splunk Cloud console. `Click the Splunk cloud logo` top left-hand side. 
2. Next `click on Search & Reporting` app on the left-hand menu. 
3. `Adjust your timerange picker to last 15 minutes`
4. `Run the following search` to view our web access logs

```
index="access_combined"
```

See example output below. 

![image_tag](/static/lab5/access_combined_confirmed_ddas.png)

Now that we have confirmed that. Let's create a MDL raw storage destination to route our data to. 

### Creating a new Machine Data Lake Dataset ###

5. `Click the Splunk Cloud logo` again on the top left-hand side. 
6. `Click on Data Management app` on the left-hand menu. 
7. `Click the Datasets option` on the left-hand menu.

>[!NOTE]
>From here we have can see all of the datasets from previous labs. Let's create our second MDL catalog.  

8. `Click the blue Create dataset button`

Like before there are lots of options here. `Select the Machine Data Lake (MDL) option.` See example below

![image_tag](/static/lab5/select-mdl-dataset-option.png)

9. `Click Next` when ready. 

10. Similar to creating other datasets, we need to enter important information for our MDL dataset. `Fill in the details as required below.`

- **Dataset Name:** access_combined_mdl
- **Dataset description:** Machine Data Lake location that will be used to store low utilised access_combined data. 
- **Data retention options:** leave as is
- **Promotion Limits:** leave as is
- **Max rolling window:** leave as is
- **Max allowed promotion volume:** leave limited as toggled
- **Volume limit:** leave as 1GB

See example filled out MDL dataset definition:

![image_tag](/static/lab5/mdl-define-dataset-filled-in-access-combined.png)

11. `Click next` when ready. 

Like before we want to add access to our sc_admin user to access the MDL raw table we are creating. 

12. On the configure permissions screen, `click the dropdown and select the sc_admin role.`
13. Next to the role. `Click the + button next to sc_admin.` See example below
14. From the drop down `select the Streaming promote, static promote, query and view option.`

Once selected your screen should look like this:

![image_tag](/static/lab5/select-plus-sc-admin-role.png)

13. `Click Next` when ready. 

Last time we create our MDL catalog we configured a sourcetype of Linux_secure so any data that came in with Linux_secure was automatically routed to the catalog. This time we are doing to send data directly to MDL catalog for access_combined via the HEC token. Because of this we don't need to define it here in the MDL setup. 

14. `Leave define data landing empty` and `Click next.`

17. Review the summary screen and `click Create Dataset` when ready. 

Once created you should now see your new MDL dataset in the list. See example below:

![image_tag](/static/lab5/access-combined-dataset-mdl-created.png)

Now we need to get data going into it. But before we do that, let's confirm it is indeed bringing in no data at all

### Validate Data is NOT flowing into MDL ###

1. From your Splunk Cloud console. `Click the catalog icon near the top right hand side` as shown in previous step. See example below:

![image_tag](/static/lab5/catalog-shortcut-button.png)

We should be able to see our new access_combined_mdl RAW table in the list. See example below: 

![image_tag](/static/lab5/splunk-catalog-screen-access-combined.png)

3. `Select the new access_combined_mdl table` from the list. 

Now we should see a summary of our new MDL table. See example summary below: 

![image_tag](/static/lab5/mdl-table-summary-catalog-access-combined.png)

4. `Click the search button` to open up the search page for the MDL table. 

You will see a promotion warning appear. See example below: 

![image_tag](/static/lab4/expensive-search-warning.png)

5. `Click the blue search button` on the warning screen to proceed. 

By default the search will already appear as a SPL2 query with a limit of 10 results. You should see No results yet found. See example below: 

![image_tag](/static/lab5/mdl-search-result-access-combined-no-results.png)

>[!NOTE]
>This has just confirmed we don't have any data coming in to the MDL. Currently its a giant container waiting for data. 

Let's now route data into it via a different method than what we did before. 

### Route Data into MDL via HEC ###

1. `Click the cog icon near the top right-hand side.`
2. Scroll down and `select Data inputs` under the DATA section. 
3. `Select HTTP Event Collect` link. 

>[!IMPORTANT]
>Your lab environment may look different to mine. So keep that in mind with screen shots! 

You will see some HEC token here in the list. There should be one for our access_combined data. 

4. Select the access_combined HEC token link as shown below:

![image_tag](/static/lab5/select-access-combined-hec.png)

You will see a few options here on the screen. Here we want to adjust the index where the data is being routed too.

>[!TIP]
>HEC allows you to assign multiple indexes to a single HEC token. This is basically saying which indexes does this HEC token have permission to send data too.

5. In the section which says available indexes, `find and select our new ~.federated_access_combined_mdl index.`

This should move it from the available indexes to the Selected indexes section. 

6. `Click the access_combined index on the Selected indexes.`

This should now do the reverse and move the original access_combined DDAS (tsidx) index off. 

>[!NOTE]
>You may see the default index section change when you de-select the access_combined index. This is because there is now only one index in the selected indexes so the default changed to that. 

See example screen of HEC token configuration changes. 

![image_tag](/static/lab5/updated-hec-token-access-combined.png)

7. `Click Save` when ready.

Ok, that should now be routing data to the MDL catalog for Access_Combined and no longer routing data to the original access-combined index. Let's test it. 

### Validating our data routing to MDL catalog and no longer going to access_combined DDAS index ###

Let's first validate data is no longer going to DDAS. 

1. Go to the Splunk search an reporting app by `clicking the Splunk Cloud icon` top left-hand side. 
2. `Click the search & reporting` app on the left-hand menu. 
3. `Run the following search below` which will show us last 5 minutes of searches on access_combined. 

```
index=access_combined earliest=-5m
```

We should see now results. See example below: 

![image_tag](/static/lab5/data-no-longer-in-access-combineed-tsidx.png)

>[!NOTE]
>The original data up until we stopped it coming in will still be in the index. Something to keep in mind. You could test this by adjusting that earliest=-5m to something like earliest=-1h

Next let's validate data is going into our new MDL catalog for access_combined. 

4. `Select the catalog icon` again from the near top right-hand side. 

5. Again, `select the access_combined_mdl table`

6. `Click the search button` again, followed by the `search button again on the warning screen.`

If all went to plan you should now see data coming into the catalog we created! 

Congratulations, that wraps up Lab 5! Now onto our last lab which we look at configuring a Dynamic Data Self Storage archive index using Amazon S3. 

### Click <a>[next](/content/lab6_configuring_fs_ddss/index.en.md)</a> to start the next lab on with lab. 





