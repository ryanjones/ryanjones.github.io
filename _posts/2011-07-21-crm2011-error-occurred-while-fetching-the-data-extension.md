---
title: 'CRM2011 Reports - Error occurred while fetching the data extension'
categories: [Microsoft]
---


I went to upload a report into CRM 2011 and this error came up:
>Error occurred while fetching the data extension
Error Details: Error occurred while fetching the list of data extensions installed on the report server.

I also ran into this error while doing an import of reports into an organization (on a different occasion)
>Error occurred while fetching the report

![][2]

 [2]: /assets/img/old/Reporting-error-Error-occurred-while-fetching-the-data-extension.png

I first check that all of my SSRS 2008 R2 services were started. They seemed okay. From there I went over to the CRM server’s event log and found this:

>Web service request ListExtensions to Report Server http://crm.contoso.com/ReportServer/ReportService2005.asmx failed. Error: The request failed with HTTP status 404: Not Found.

From this error I can infer that CRM can’t find my reporting services. That path didn’t even exit (when I try and go to it). At this point I clued in that I had changed the SSRS’s virtual directory earlier on. I changed it to ReportServer_MSSQL (for other reasons). Here’s what it looks like:

![][3]

 [3]: /assets/img/old/Reporting-error-Change-Virtual-Directory.png

Now we need to change the SQL Reporting Services URL for our organization. To do this:

*   Open Deployment Manager
*   Disable your organization
*   Click Edit Organization (right hand side)
*   Update your SSRS URL virtual directory (mine was http://crm.contoso.com/ReportServer_MSSQL/ReportService2005.asmx)
*   Click Next, Next and then Finish
*   Enable your organization

![][4]

 [4]: /assets/img/old/Reporting-error-Editing-CRM2011-Org.png

I was now able to go into CRM2011 and update the reports. Hopefully this helps!
