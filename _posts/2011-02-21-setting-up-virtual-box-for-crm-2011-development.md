---
title: Setting up Virtual Box for CRM 2011 Development
categories: [Microsoft]
---


Here’s how we can setup the base image for the CRM installation.

Download and install virtual box from here: [VirtualBox][1]

 [1]: http://www.virtualbox.org/wiki/Downloads

Click New -> Click Next on the Terms and Conditions -> Give the image a name (MS Server 2008 R2 w/CRM 2011 – 64 bit)

![][3]

 [3]: /assets/img/old/VB_Setup_New2.png

Since you’re running Server 2008 R2, SQL Server 2008 R2 and CRM 2011 you’re going to need a substantial amount of ram. I use about 4 gigs. Click Next after picking the amount of RAM.

![][4]

 [4]: /assets/img/old/VM_Setup_RAM.png

Create new Hard Disk (leave Boot Hard Disk checked) -> Click Next

Click Next on the New Virtual Disk Wizard Info Screen

![][5]

 [5]: /assets/img/old/VM_Setup_HD_Storage.png

We want dynamically expanding storage -> Click Next

20 gigs is the default choice, but that’s cutting it close. I choose about 50 gigs for some extra space -> Click Next

![][6]

 [6]: /assets/img/old/VM_Setup_Location_Size.png

Click Finish on Summary

Click Finish on the Create new Virtual Machine Summary

The virtual machine is ready for use!

Let’s go set up [Server 2008 R2][7].

 [7]: http://www.ryanonrails.com/2011/02/21/server-2008-r2-setup-installation-for-crm-2011-development/