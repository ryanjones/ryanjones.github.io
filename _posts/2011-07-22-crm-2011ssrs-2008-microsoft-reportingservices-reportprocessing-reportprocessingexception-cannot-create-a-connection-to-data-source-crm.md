---
title: 'CRM 2011/SSRS 2008 - Microsoft.ReportingServices.ReportProcessing. ReportProcessingException: Cannot create a connection to data source &#8216;CRM&#8217;.'
categories: [Microsoft]
---


While using a plugin that uses NetworkCredentials I ran into this issue:

> Microsoft.ReportingServices.ReportProcessing.ReportProcessingException: Cannot create a connection to data source ‘CRM’.

There are many reasons for this error (not enough privileges to the datasource, incorrect datasource setup, etc) but this time it was caused by not having my user within CRM with enough privileges.

I added the user to CRM and assigned him the System Administrator Role (during development, I’ll probably knock it down a notch or two once in prod) and I was good to go.

**Edit June 12, 2012**
To be more clear about above.. I was using a user to run an SSRS report, and the user hadn’t been added to CRM. You need to make sure that the user that you’re “acting” as has access to the database. I was able to deduce this from the SSRS error logs (which gave me “User Was Not Found —>”).
