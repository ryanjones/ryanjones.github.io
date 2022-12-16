---
title: 'One or more datasource credentials required to run the report have not been specified - SoapException'
categories: [Microsoft]
---


I kept getting the error:

>One or more datasource credentials required to run the report have not been specified

when I was trying to run an SSRS report. Turns out I needed to set the MSCRM_FetchDataSource to Windows Integrated Security (as that’s how I was accessing the reports).
Must have missed that when I was setting up SSRS!

**Edit May 30th/2012**

I’ve decided to update this post as I feel it might be a little misleading. By default

MSCRM_FetchDataSource should be set to **Credentials supplied by the user running the report by default**. If you don’t have it set to this the out of box reports won’t run.

My plugin was using **Windows integrated security** as it’s authentication type, however if it’s set to **Credentials supplied by the user running the report by default**.

So I actually ended up going with **Credentials stored securely in the report server** with **Use as Windows credentials when connecting to the data source** checked, along with specifying a user. This allowed the out of box, and the CRM plugin to run (as the auth was stored on the server).
