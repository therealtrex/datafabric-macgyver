## Section Two: Configuring a DDSS archive index ##

In this final part of our workshop we will edit the PII index for our Credit Card transactions to assign the new DDSS archive to it. 

### Editing a Splunk Index for DDSS ###

1. Splunk your Splunk console. `Click the Cog icon located near the top right-hand side.` 
2. Scroll down and `select Indexes` under the DATA section. 

>[!NOTE]
>The indexes shown in the picture below maybe different to what you see depending on what part of the workshop you are doing in what order. 

3. We should see all of our indexes from the workshop here. Click the cc_trans_pii index as shown below:

![image_tag](/static/lab6/select-pii-index.png)

4. `Modify the Max raw data size to be 100MB (**NOT**GB!)`

5. `Click the drop down where it says additional storage.`

6. `Select Dynamic Data Self-Storage (DDSS)` option as shown below:

![image_tag](/static/lab6/select-ddss-optoin-index.png)

7. `Change searchable days as 1`

8. `Click the drop down under Self-storage location and select our new self storage location we created earlier.` See example below:

![image_tag](/static/lab6/ddss-archive-location-selected-on-index.png)

9. `Click Save when done.`

10. `Click continue if prompted for updating index.`

We should now see on the right hand side of our index the DDSS setup location showing. See example below:

![image_tag](/static/lab6/ddss-location-setup-on-index-final-screen.png)

Just as a summary. We modified the index to route data to Splunk DDSS in Amazon S3 when it hits 100MB or 1 Day old. These are the smallest numbers Splunk will allow. 

<br>

>[!IMPORTANT]
>Unfortunately we cannot show you the live feed of archive working as we will have to wait a day for it to archive off. Feel free to set a reminder in your diary to check in around 24 hours. The labs usually last another 1 to 2 days after the workshop. 

That is it! You have completed all of the labs. I hope you enjoyed your experience in getting your hands dirty with Cisco Data Fabric. Have a great day. 