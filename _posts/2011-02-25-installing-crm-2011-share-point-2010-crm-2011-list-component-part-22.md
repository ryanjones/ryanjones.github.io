---
title: 'Installing CRM 2011 / Share Point 2010 / CRM 2011 List Component - Part 2/2'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Now onto the nitty gritty! Let’s get CRM 2011 / Share Point 2010 / CRM 2011 List Component installed and configured.

Some notes:

I probably should have picked a CRM 2011 port during the install so CRM 2011 and Share Point 2010 wouldn’t have clashed (I worked through it, but it would have been nice not to have to change the port (I learned something though, right?)).

The “fix” for the upload control I’m pretty sure is a bunch of hooey. According to the article it says that just by using machinename:port vrs localhost:port it should work. It didn’t work for me so I tried whatever I could to get it fixed. In the end I just deleted those bindings (when trying to fix the web services) and the uploading to Share Point 2010 worked perfectly.

If you see something odd, or want to add some input please post something. I do monitor my blog and I’d love to help out wherever I can.

Installing / Configuring SharePoint 2010:  
http://www.ryanonrails.com/2011/02/25/installing-configuring-sharepoint-2010/

Changing the CRM Server Port:  
http://www.ryanonrails.com/2011/02/25/changing-the-crm-server-port/

Creating a Web Application and Share Point 2010 Site:  
http://www.ryanonrails.com/2011/02/25/creating-a-web-application-and-share-point-2010-site/

Creating a Share Point 2010 Site Collection:  
http://www.ryanonrails.com/2011/02/25/creating-a-share-point-2010-site-collection/

Hooking CRM 2011 and Share Point 2010 together:  
http://www.ryanonrails.com/2011/02/25/hooking-crm-2011-and-share-point-2010-together/

Changing CRM 2011 Web Services Port via Deployment Manager:  
http://www.ryanonrails.com/2011/02/25/changing-crm-2011-web-services-port-via-deployment-manager/

Drop me a line via Twitter or send an email for the quickest response.

Enjoy CRM 2011 & Share Point 2010 magic!