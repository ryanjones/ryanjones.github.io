---
title: 'SSRS 2008/CRM 2011 - System.InvalidOperationException: Trace Directory not defined'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Whenever I ran a report in Reporting Services 2008 I got this error:

>processing!ReportServer_0-1!2620!07/14/2011-09:49:30:: e ERROR: Throwing Microsoft.ReportingServices.ReportProcessing.ReportProcessingException: , Microsoft.ReportingServices.ReportProcessing.ReportProcessingException: Cannot create a connection to data source ‘CRM’. —> System.InvalidOperationException: Trace Directory not defined

It didn’t matter which report I always got this error. I double checked all of my permissions and just couldn’t figure out why it couldn’t find a trace directory. I ended up stumbling across this [link][1] which basically said to uninstall and reinstall the CRM Data Connector.

 [1]: http://social.microsoft.com/Forums/en/crm/thread/30aaa0a3-79a7-4054-995c-233901a9cfb7 "http://social.microsoft.com/Forums/en/crm/thread/30aaa0a3-79a7-4054-995c-233901a9cfb7"

I couldn’t believe it but it actually worked. Just uninstall the CRM Data Connector and then install it again.

Some things are mystifying!
