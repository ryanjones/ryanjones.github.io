---
title: 'Microsoft.Crm.CrmConfigObjectNotFoundException: User Was Not Found'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Whenever I ran a report in SSRS I kept getting:

> Microsoft.Crm.CrmConfigObjectNotFoundException: User Was Not Found

It’s weird because I was able to login to CRM with that user account. It turns out that the user had been deleted from AD then re-added (without my knowledge). I disabled the current user in CRM and re-added the user to CRM and everything worked just fine from there on out. It’s almost like the old user’s GUID wasn’t matching with the new user’s GUID.
