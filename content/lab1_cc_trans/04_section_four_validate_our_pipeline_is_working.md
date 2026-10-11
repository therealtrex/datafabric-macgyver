## Section Four: Validating our credit card transactions pipeline in Splunk

Now we have applied our pipeline here is what we should be seeing now. 
- First our cc_trans_im index is showing our dropped and redacted credit card transaction details. 
- Second our cc_trans_pii index will show us the original un-changed raw data. 

Lets validate! 

### Validate pipeline in Splunk ###

1.	From our splunk console. `Set the Time range from last 15 minutes` and `paste the following code in` and run the search to check our cc_trans_im index data has changed. See example result below:

```
index="cc_trans_im"
```

![image_tag](/static/lab1/cc_trans_im_changed.png)


You can see our cc_trans_im index has indeed changed. The data is less and the CC_NUMBER is ‘xxx’ out! 

2.	Lastly, either in a new tab or replace the current search with the `following SPL code below` and run a new search. See example result below: 

```
index="cc_trans_pii"
```

![image_tag](/static/lab1/original_credit_card_new_index.png)


Congratulations you have completed the first part of the workshop by splitting our data into two indexes. One with masked data for our monitoring team and the other for our PII auditors. 

Well done! Time to move onto Lab 2!

### Click <a>[next](/content/lab2_cisco_firewall_logs/index.en.md)</a> to go kick off lab2! 

