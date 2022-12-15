---
title: Creating a Domain Controller
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


CRM needs a domain controller (to manage users/auth essentially) so let’s convert our server into a domain controller.

Click Start -> Run, and type in “dcpromo” then hit enter. What should come up is a Active Directory Domain Services Installation Wizard. Click Next.

![][2]

 [2]: /assets/img/old/DC_dcpromo.png

On the Operating System Compatibility screen, click Next

On the Choose a deployment configuration, click “Create a new domain in a new forest”, then click Next.

![][3]

 [3]: /assets/img/old/DC_Forst_Level.png

Now we need to enter in a FQDN (a .com). As this is just a dev box, punch in corp.contoso.com, click Next.

[!DC FQDN][4]

 [4]: /assets/img/old/DC_FQDN.png

Set forest functionality level to “Windows Server 2008 R2”, click Next.

![][5]

 [5]: /assets/img/old/DC_Forst_Level.png

Additional Domain Controller Options, Make sure DNS Server is checked, click Next

![][6]

 [6]: /assets/img/old/DC_DNS.png

In a normal world you’d want to set a static ip on a domain controller, again, since this is a dev box, I just go with an IP from the DHCP server. Click Yes.

![][7]

 [7]: /assets/img/old/DC_DHCP.png

If you get this error, click next.

![][8]

 [8]: /assets/img/old/DC_Delegation.png

For the Locations of the Database, Log Files and SYSVOL, leave the defaults and click Next.

Punch in an admin password to use (I use pass@word1), click Next

At the Summary page, click Next.

Once the configuration is complete, Reboot the PC.

At this point you should take another snapshot and name it “After dcpromo”.

Let’s get [CRM 2011 Installed][9].

 [9]: http://www.ryanonrails.com/2011/02/21/installing-crm-rtm-2011-for-development/