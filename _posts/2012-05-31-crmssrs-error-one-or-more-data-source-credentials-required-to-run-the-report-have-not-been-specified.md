---
title: 'CRM/SSRS - Error: One or more data source credentials required to run the report have not been specified.'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---

I ran into this error again. I think I’ve got it all figured out. Here’s the excerpt from the log:

> Unhandled Exception: System.ServiceModel.FaultException`1[[Microsoft.Xrm.Sdk.OrganizationServiceFault, Microsoft.Xrm.Sdk, Version=5.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35]]: An exception occurred when executing the plugin. Error: One or more data source credentials required to run the report have not been specified. —> Microsoft.ReportingServices.ReportProcessing.ReportProcessingException: One or more data source credentials required to run the report have not been specified. 

My current setup is this. I have regular MSCRM_FetchDataSource reports using **Credentials supplied by the user running the report**. I’m currently running a plugin when a a drop down list value has changed. This plugin runs an SSRS report and drops it into sharepoint.

The error actually stems from the fact that the username and password being passed to SSRS from the plugin isn’t getting populated. The SRS data connector that you install for CRM passes these values back and for you for the out of box reports. For whatever reason, this code will not populate those fields:

```csharp
return ReportService.RenderReport(reportUrl, _networkCredential, reportPath, parameters, "PDF", devInfo, "en-us");
```

In this situation I’ve found the best course of action is to set your data source as follows:

MSCRM_FetchDataSource  
Credentials stored securely in the report server  
User:-username-  
Password:-password-  
Use as Windows credentials when connecting to the data source **CHECKED**  
Impersonate the authenticated user after a connection has been made to the data source **NOT CHECKED**

We use 1 main account for our reports so this works out well. This will allow plugins to access SSRS and allow the out of box reports to run. All while only having to use 1 datasource (we previously had the custom reports on 1 datasource, and the rest of CRM on MSCRM_FetchDataSource).