## Section One: Creating our first MDL table

In section one we are going to validate the data and then create our first MDL table which we can configure to intercept logs coming into DDAS as required. 

### Validating the problem ###

First part of this lab is to check the data coming into Splunk is actually hitting the DDAS index. 

1. From your Splunk Cloud console. `Click the Splunk cloud logo` top left-hand side. 
2. Next `click on Search & Reporting` app on the left-hand menu. 
3. `Adjust your timerange picker to last 15 minutes`
4. `Run the following search` to view our web access logs

```
index="linux_secure"
```

See example output below. 

![image_tag](/static/lab4/linux_secure_confirmed_ddas.png)

Now that we have confirmed that. Let's create a MDL raw storage destination to route our data to. 

### Creating a new Machine Data Lake Dataset ###

5. `Click the Splunk Cloud logo` again on the top left-hand side. 
6. `Click on Data Management app` on the left-hand menu. 
7. `Click the Datasets option` on the left-hand menu.

>[!NOTE]
>From here we have already been in previous labs that there is a default_mdl_raw dataset already. This was created as part of the lab. But for this lab we want to create a new one to route our linux_secure data too. 

8. `Click the blue Create dataset button`

Like before there are lots of options here. `Select the Machine Data Lake (MDL) option.` See example below

![image_tag](/static/lab4/select-mdl-dataset-option.png)

9. `Click Next` when ready. 

10. Similar to creating other datasets, we need to enter important information for our MDL dataset. `Fill in the details as required below.`

- **Dataset Name:** `linux_secure_mdl`
- **Dataset description:** `Machine Data Lake location that will be used to store low utilised linux_secure data.` 
- **Data retention options:** `leave as is`
- **Promotion Limits:** `leave as is`
- **Max rolling window:** `leave as is`
- **Max allowed promotion volume:** `leave limited as toggled`
- **Volume limit:** `leave as 1GB`

See example filled out MDL dataset definition:

![image_tag](/static/lab4/mdl-define-dataset-filled-in.png)

>[!NOTE]
>We covered off quite a few options there, we don't have time to go through which options we chose for what reason but all the information you need can be found <a>[HERE](https://help.splunk.com/en/data-management/onboard-data-to-splunk-cloud-platform/land-data-in-machine-data-lake/1.0/land-and-verify-data/create-a-machine-data-lake-raw-table)</a>

11. `Click next` when ready. 

On the next screen you can get granular around who has access to perform actions on the dataset. For simplicity we are just going to allow access to Admin for this lab but you can find more information on setting user permissions on MDL <a>[HERE](https://help.splunk.com/en/data-management/onboard-data-to-splunk-cloud-platform/land-data-in-machine-data-lake/1.0/prepare-for-onboarding/access-control-roles-and-capabilities)</a>

12. On the configure permissions screen, `click the dropdown and select the sc_admin role.`
13. Next to the role. `Click the + button next to sc_admin.` See example below
14. From the drop down `select the Streaming promote, static promote, query and view option.` See example below

Once selected your screen should look like this:

![image_tag](/static/lab4/select-plus-sc-admin-role.png)

13. `Click Next` when ready. 

This screen looks just like a ingest processor pipeline wizard. Well it probably is. But anyway. This is where we can select how the MDL will process the data coming in and select which data lands in the machine data lake raw table we are creating. 

14. `Click the partition by sourcetype` option. 

15. `Copy and paste the below into the Value field`

```
linux_secure
```

16. `Click Apply when ready`

Your process_data section should look like this: 

![image_tag](/static/lab4/mdl-process-data-by-sourcetype.png)

17. `Click next when ready`

17. Review the summary screen and `click Create Dataset` when ready. 

Once created you should now see your new MDL dataset in the list. See example below:

![image_tag](/static/lab4/new-mdl-dataset-created.png)

Nice work! Now we can check if data is flowing into it!

### Validate Data is flowing into MDL ###

1. From your Splunk Cloud console. `Click Cog icon near the top right-hand side`. 

2. From there `select Catalog` option. This should be under the section called Knowledge. 

This is our new Catalog service inside of Splunk. From here you can see all of the different indexes and machine data lake catalogs. We should be able to see our new linux_secure_mdl RAW table in the list. See example below: 

![image_tag](/static/lab4/splunk-catalog-screen.png)

3. `Select the new linux_secure_mdl table` from the list. 

Now we should see a summary of our new MDL table. See example summary below: 

![image_tag](/static/lab4/mdl-table-summary-catalog.png)

>[!NOTE]
>Clicking view hamburger (three dots) next to search will take you to the section where you can get more information on the configured table and edit it as well. But let's stay focused for now. 

4. `Click the search button` to open up the search page for the MDL table. 

You will see a promotion warning appear. See example below: 

![image_tag](/static/lab4/expensive-search-warning.png)

5. `Click the blue search button` on the warning screen to proceed. 

>[!NOTE]
>Search results may take up to 10 minutes to start appearing. 

By default the search will already appear as a SPL2 query with a limit of 10 results. You should see some results from our recently created MDL table. See example below: 

![image_tag](/static/lab4/mdl-search-result-linux-secure.png)

>[!IMPORTANT]
>As we have configured the MDL table to route all data matching linux_secure sourcetype, this will mean that we are only sending data to MDL raw now in our new table. 

Let's verify we no longer are seeing linux_secure coming into our DDAS storage. 

6. `From a new tab open up the Splunk search bar again.` You can do this by clicking the Splunk cloud logo on the top left-hand side and selecting the Search & Reporting app. 
7. `Run the following SPL search` against our original linux_secure index. 

```
index=linux_secure earliest=-2m
```

>[!NOTE]
>You will notice we used the earliest=-2m command again to quickly check no new data is coming in. 

See example of no results in our search now

![image_tag](/static/lab4/no-results-after-routing-to-mdl-splunk-search.png)


We have now configured the MDL table to take all of the linux secure data and we don't have to worry about it going to DDAS anymore. But what if we wanted to promote a set of that data back to DDAS? Well that is the next part of the lab. 

### Click <a>[next](/content/lab4_machine_data_lake_101/02_promote_data_mdl.md)</a> to continue on with lab. 





