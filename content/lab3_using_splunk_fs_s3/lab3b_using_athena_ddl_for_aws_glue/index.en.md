---
title: Lesson Two - VPC Flow Logs
weight: 30
---

## Overview

Welcome to lesson two. Now we understand the basics of Splunk's Federated Search for Amazon S3 it's time to take a look at another use case and learn some new tricks. For this use case we will explore VPCFlow Logs.

After this lesson you will get an understanding of the following:

1. **Be familiar with VPC Flow logs**: Take a quick look at how to configure VPC flow logs by spinning up an Amazon Ec2 and configuring the logs to land in our favorite S3 bucket.

2. **Searching different Log formats**: We will use a binary column format of parquet for log style in this lesson to see how it works with Federated Search. 

3. **Take a look at Amazon Athena**: Learn how Amazon Athena help with creating / modify AWS Glue Tables as well as help run queries also against S3. This can help debug and test out things before running them in Splunk.

4. **Add a second Federated Dataset**: We will create another federated dataset in Splunk.

Below is overview of the architectural pieces we will be reviewing in this lesson.

![Workshop Architecture](/static/images/lesson2/architecture-lesson2.png)

When ready **Click Next** to begin the lesson.
