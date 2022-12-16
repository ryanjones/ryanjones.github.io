---
title: Error Validating SharePoint Site while using CRM2011 with SSL
categories: [Microsoft]
---


I recently received this error when trying to validate a SharePoint site with the list component: The URL schemes of Microsoft Dynamics CRM and SharePoint Server are different. This error prevented me from validating the SharePoint site.

Basically I was on the CRM site https://CRMexamplesite.com and trying to setup the sharepoint site http://SPexamplesite.com. It didn’t like the fact that I was using SSL on 1 machine and not the other.

I didn’t want to set up SharePoint as SSL right now (we still needed to order our certificate) so I logged onto the local machine that hosted CRM and connected to http://CRMexamplesite.com which then allowed me to add the SharePoint site http://SPexamplesite.com.