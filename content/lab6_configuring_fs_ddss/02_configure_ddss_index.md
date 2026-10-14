## Section Two: Configuring a DDSS archive index ##

In this final part of our workshop we will edit the PII index for our Credit Card transactions to assign the new DDSS archive to it. 

### Editing a Splunk Index for DDSS ###

1. Splunk your Splunk console. `Click the Cog icon located near the top right-hand side.` 
2. Scroll down and `select Indexes` under the DATA section. 

>[!NOTE]
>The indexes shown in the picture below maybe different to what you see depending on what part of the workshop you are doing in what order. 

3. We should see all of our indexes from the workshop here. Click the cc_trans_pii index as shown below:

![image_tag](/static/lab6/select-pii-index.png)

4. `Click the drop down where it says additional storage.`

5. `Select Dynamic Data Self-Storage (DDSS)` option as shown below:

![image_tag](/static/lab6/select-ddss-optoin-index.png)

6. `Leave searchable days as 100`

7. `Click the drop down under Self-storage location and select our new self storage location we created earlier.` See example below:

![image_tag](/static/lab6/ddss-archive-location-selected-on-index.png)

8. `Click Save when done.`

9. `Click continue if prompted for updating index.`

We should now see on the right hand side of our index the DDSS setup location showing. See example below:

![image_tag](/static/lab6/ddss-location-setup-on-index-final-screen.png)

<br>

>[!IMPORTANT]
>Unfortunately we cannot show you anymore form here as the minimum we can set for the DDSS archival process to work is 1 day retention. Seel free to set this yourself if you have time left in the lab and check it the following day. 

That is it! That is all the labs. I hope you enjoyed the labs we put together and learn a lot. Have a great day. 