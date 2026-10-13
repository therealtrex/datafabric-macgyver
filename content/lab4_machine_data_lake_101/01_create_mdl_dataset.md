## Section One: Routing Data to Splunk Machine Data Lake from Ingest Processor

Section one of Lab 4 is configuring our Ingest Processor to intercept web access log data currently going to our Splunk DDAS high performance storage and route it to Splunk Machine Data Lake instead. 

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

![image_tag](/static/lab2/linux_secure_confirmed_ddas.png)

Now that we have confirmed that. Let's create a MDL raw storage destination to route our data to. 

### Creating a new Machine Data Lake Dataset ###

5. `Click the Splunk Cloud logo` again on the top left-hand side. 
6. `Click on Data Management app` on the left-hand menu. 
7. `Click the Datasets option` on the left-hand menu.

>[!NOTE]
>From here we have already been in previous labs that there is a default_mdl_raw dataset already. This was created as part of the lab. But for this lab we want to create a new one to route our linux_secure data too. 

8. `Click the blue Create dataset button`

Like before there are lots of options here. `Select the Machine Data Lake (MDL) option.` See example below

![image_tag](/static/lab2/select-mdl-dataset-option.png)

9. `Click Next` when ready. 

10. Similar to creating other datasets, we need to enter important information for our MDL dataset. `Fill in the details as required below.`

- **Dataset Name:** linux_secure_mdl
- **Dataset description:** Machine Data Lake location that will be used to store low utilised linux_secure data. 
- **Data retention options:** leave as is
- **Promotion Limits:** leave as is
- **Max rolling window:** leave as is
- **Max allowed promotion volume:** leave limited as toggled
- **Volume limit:** leave as 1GB

See example filled out MDL dataset definition:

![image_tag](/static/lab2/mdl-define-dataset-filled-in.png)

>[!NOTE]
>We covered off quote a few options there, we don't have time to go through which options we chose for what reason but all the information you need can be found <a>[HERE](https://help.splunk.com/en/data-management/onboard-data-to-splunk-cloud-platform/land-data-in-machine-data-lake/1.0/land-and-verify-data/create-a-machine-data-lake-raw-table)</a>

11. `Click next` when ready. 

On the next screen you can get granular around who has access to perform actions on the dataset. For simplicity we are just going to allow access to Admin for this lab but you can find more information on setting user permissions on MDL <a>[HERE](https://help.splunk.com/en/data-management/onboard-data-to-splunk-cloud-platform/land-data-in-machine-data-lake/1.0/prepare-for-onboarding/access-control-roles-and-capabilities)</a>

12. On the configure permissions screen, `click the dropdown and select the Admin role.`

>[!NOTE]
>You will have to click away to close the drop down again. 

See example below of admin role selected:

![image_tag](/static/lab2/admin-role-selected-user-permissions-mdl-dataset.png)

13. `Click Next` when ready. 

>[!IMPORTANT]
> Machine Data Lake has quite a few ways you can ingest data into it. Some of these can be automatically pushes of data, eg HEC and some are manually triggered pushes as part of an ingest processor pipe as an example. For our lab we wil use ingest processor to intercept and route data to our new dataset. So we can ignore this section of the dataset creation process. 


14. Leave the define data landing page and `Click next` to continue. 

15. Review the summary screen and `click Create Dataset` when ready. 

Once created you should now see your new MDL dataset in the list. See example below:

![image_tag](/static/lab2/new-mdl-dataset-created.png)

Nice work! We now have our new MDL dataset. In the next section we will go to Ingest Processor and route our data to it. 

### Click <a>[next](/content/lab4_machine_data_lake_101/02_setup_ingest_processor_route_mdl.md)</a> to continue on with lab. 





