---
title: Cannot connect with Document Manager via CRM 2011 to Sharepoint 2010
categories: [Microsoft]
---


I recently ran into this error **AGAIN** while trying to run the Document Manager in CRM 2011: **“An error occurred while loading the page. The URL may not have been mapped in the SharePoint Server. Ask your system administrator to check the Configure alternate access mappings settings in the SharePoint Central Administration.”**

And in the SharePoint server’s event viewer I found the following error: **“Alternate access mappings have not been configured. Users or services are accessing the site http://crmserver:5555 with the URL https://sp.crmserver.ca. This may cause incorrect links to be stored or returned to users. If this is expected, add the URL https://sp.crmserver.ca as an AAM response URL. For more information, see: http://go.microsoft.com/fwlink/?LinkId=114854″/>”**

First, make sure the site browsing is permissive. Now we need to setup our alternate access mappings in SharePoint.

*   Head over to your SharePoint central administration
*   Click Application Management
*   Click Configure alternate access mappings
*   This menu is kind of funky, beside Alternate Access Mapping Collection: Change Alternate Access Mapping Collection (top leftish), click your site (which should load and show Alternate Access Mapping Collection: )
*   Click Edit public URLS

![][2]

 [2]: /assets/img/old/Alternate_Access_Mappings.png

Default would be http://crmserver:5555 and internet would be https://sp.crmserver.ca. This would be needed if you’re using external DNS. If you don’t set this up it tries to use the internal address to communicate out, which of course causes the connection to fail.

![][3]

 [3]: /assets/img/old/Alternate_Access_Mappings_URLs.png

Once the alternate mappings were setup I was able to continue on through the Document Manager.

Have fun!
