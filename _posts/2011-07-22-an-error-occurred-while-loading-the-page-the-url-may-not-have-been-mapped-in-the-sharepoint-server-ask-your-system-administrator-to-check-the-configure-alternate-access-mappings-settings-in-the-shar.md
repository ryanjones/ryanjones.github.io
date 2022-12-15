---
title: >
  An error occurred while loading the page. The URL may not have been mapped in
  the SharePoint Server. Ask your system administrator to check the Configure
  alternate access mappings settings in the SharePoint Central Administration.
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


I recently ran into this error while trying to run the Document Manager in CRM 2011:

>An error occurred while loading the page. The URL may not have been mapped in the SharePoint Server. Ask your system administrator to check the Configure alternate access mappings settings in the SharePoint Central Administration.

It was bizarre because I previously was working with the SharePoint 2010 site (with the list component control) without any issues. Here’s what I did to fix it:

*   Go to SharePoint 2010
*   Deactivate the solution (crmlistcomponent)
*   The deactivation went successful, but when I went to activate it again I was then greeted with the error “No available sandboxed code execution server could be found”.
*   From there I went to the Central Administration server (SharePoint 2010) -> System Settings -> Manage services on server -> Stop Microsoft SharePoint Foundation Sandboxed Code Service -> and then Start Microsoft SharePoint Foundation Sandboxed Code Service
*   I then went and activated the solution (without errors)

At some point I restarted the CRM Sandbox service. I don’t think it had anything to do with the fix, but I figured it might be worth mentioning it if people are still having issues. Hopefully this gets you off and running!


**Edit Aug 03,2011** – If this doesn’t work head over to [http://www.ryanonrails.com/2011/08/03/cannot-connect-with-document-manager-via-crm-2011-to-sharepoint-2010/][1] which might help you solve your problem.

 [1]: http://www.ryanonrails.com/2011/08/03/cannot-connect-with-document-manager-via-crm-2011-to-sharepoint-2010/ "http://www.ryanonrails.com/2011/08/03/cannot-connect-with-document-manager-via-crm-2011-to-sharepoint-2010/"