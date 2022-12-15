---
title: 'Setting up a CRM 2011 Virtual Machine (on a Domain Controller) - Part 1/2'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Hi Everyone,

Now that CRM 2011 RTM is out I’ve decided that I need a dev box so I can create some plugins and do some testing of the new features. Now I’m not sure about you, but I only like to work in 1 virtual machine. You could build a VM for Server 2008 (domain controller), SQL Server, CRM 2011, and hook it all together, but not only does that suck up all your resources, it’s annoying to have to switch between all of them.

I’ve taken pictures and notes while building a CRM 2011 image with all of the components to run it. Please let me know if you think I’ve missed anything important. Expect this to take about 3-4 hours depending on the type of machine running the installation.

Please remember, this is just for developers, or people who want to play around with CRM 2011 in 1 image. **NEVER SET THIS UP IN PRODUCTION**. I cannot stress this enough.

Interestingly enough, back in CRM 4.0, according to the documentation located [here][1] it says that “The computer on which Microsoft Dynamics CRM is running **cannot** function as an Active Directory domain controller, unless it is running Microsoft Windows Small Business Server 2003 Premium Edition R2.” However in CRM 2011 it seems to have been changed (located [here][2]) to “Microsoft Dynamics CRM **should** only be installed on a Windows Server that is a domain member or, if you are installing on Microsoft Small Business Server, a domain controller. The domain where the server is located must be running in one of the following Active Directory modes”. I didn’t get any prompts saying I couldn’t install it on a domain controller, so maybe Microsoft has had a change of heart. Thanks to [Ryan C][3] for looking this up.

 [1]: http://msdn.microsoft.com/en-us/library/dd979168.aspx
 [2]: http://technet.microsoft.com/en-us/library/gg554883(d=lightweight).aspx
 [3]: http://prodynamicscrm.com/

Onto the good stuff (follow these in order):

Setting up Virtual Box:  
http://www.ryanonrails.com/2011/02/21/setting-up-virtual-box-for-crm-2011-development/

Setting up Server 2008 R2:  
http://www.ryanonrails.com/2011/02/21/server-2008-r2-setup-installation-for-crm-2011-development/

Enabling Virtual Box Guest Additions:  
http://www.ryanonrails.com/2011/02/21/installing-guest-additions-for-an-oracle-virtual-box-machine/

Setting up SQL Server 2008 R2:  
http://www.ryanonrails.com/2011/02/21/installing-sql-server-2008-r2-for-crm-2011-development/

Creating a Domain Controller (as CRM 2011 needs one):  
http://www.ryanonrails.com/2011/02/21/creating-a-domain-controller/

Installing CRM 2011:  
http://www.ryanonrails.com/2011/02/21/installing-crm-rtm-2011-for-development/

Some of the articles will be updated in the future to provide some consistency, however I feel getting this up sooner than later might help some people out.

Feel free to post and ask questions!