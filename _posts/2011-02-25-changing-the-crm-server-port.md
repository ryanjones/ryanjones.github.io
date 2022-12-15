---
title: Changing the CRM 2011 Server Port
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


To make sure we can access the CRM Server we need to change the port. It will help us specify what it’s accessing. CRM and Share Point are pointed at port 80 right now (which is bad).

Click Start -> Run -> type in “inetmgr” and hit enter.

Click Microsoft Dynamics CRM, and then click “Stop” under Manage Web Site.

![][2]

 [2]: /assets/img/old/CRM2011_Sharepoint2010_Port_Change_15.png

Click “Bindings…” under Edit Site

Click http and click Edit. Change the port to 85 (or whatever # you want) and Click OK. Now you can see the server running under port 85. You can now go to http://localhost:85 to access CRM 2011.

![][3]

 [3]: /assets/img/old/CRM2011_Sharepoint2010_Port_Change_85_16.png

Let’s go and [create a web application and Share Point 2010 site][3].

 [3]: http://www.ryanonrails.com/2011/02/25/creating-a-web-application-and-share-point-2010-site/