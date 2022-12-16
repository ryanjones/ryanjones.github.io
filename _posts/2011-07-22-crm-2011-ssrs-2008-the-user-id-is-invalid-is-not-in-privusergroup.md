---
title: CRM 2011 / SSRS 2008 The user Id is invalid / is not in PrivUserGroup
categories: [Microsoft]
---


I had changed my CRMAppPool account (to a domain user) and started to receive these errors:

Number: 0x80041D1F
Error Message: **The user Id is invalid**.
Error Details: **The user Id is invalid**.
Source File: Not available
Line Number: Not available

Request URL: https://crm.contoso.ca/development/AppWebServices/RecentlyViewedWebService.asmx

>Stack Trace Info: [CrmException: **The user Id is invalid**.] at Microsoft.Crm.BusinessEntities.SecurityLibrary.GetPrivilegedUserCallerAndBusinessGuidsFromThread
(WindowsIdentity identity, IOrganizationContext context)

>[2011-07-14 08:21:23.115] Process:Microsoft.Crm.Sandbox.HostService |Organization:00000000-0000-0000-0000-000000000000 |Thread: 148 |Category: Sandbox |User: 00000000-0000-0000-0000-000000000000 |Level: Error | SandboxHost.AuthenticateCaller >Access Denied. Reference number for administrators or support: #15B34B57: **crm.contoso.cacrmservice is not in PrivUserGroup**

>[2011-07-14 08:21:23.130] Process:Microsoft.Crm.Sandbox.HostService |Organization:00000000-0000-0000-0000-000000000000 |Thread: 148 |Category: Exception |User: 00000000-0000-0000-0000-000000000000 |Level: Error | CrmException..ctor

The errors themselves are actually quite misleading. I can’t tell you how many times I made sure that the crmservice account was in the PivUserGroup (and for the right organization).

The actual error was because my crmservice account didn’t have the “Log on as a service” privilege on the local machine.

Here’s a link on how to set up the crmservice (CRMAppPool account) as “Log on as a service”:
[Log on as a service][1]

 [1]: http://ronaldlemmen.blogspot.com/2009/04/change-crmapppool-identity.html "http://ronaldlemmen.blogspot.com/2009/04/change-crmapppool-identity.html"

In case that blog goes defunct here’s a quote of the steps:

> Via Ronald Lemmen (who kind of looks like Ryan from “The Office”)
>
> - Create a user in Active Directory
> - Make sure the user has Domain User rights
> - Add the user to the PrivUserGroup of CRM
> - Make sure the user has ‘log on as a service’ priviledge
>
> The last step can be executed on two approaches:
> - Go to ‘Local Security Policy’ in the administrative tools (‘Domain Controller Secuirty Settings’ on a Domain Controller) and add the user to ‘Log on as a service’
> - Add the user to the CRM_WPG group in AD. This group is member of the ‘Log on as a service’ policy.

Hope this cures some headaches.