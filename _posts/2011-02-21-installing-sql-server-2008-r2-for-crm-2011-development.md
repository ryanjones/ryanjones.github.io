---
title: Installing SQL Server 2008 R2 for CRM 2011 Development
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


We’re going to need to load up the ISO/CD/DVD.

For an ISO click Devices -> CD/DVD Devices -> Choose a virtual CD/DVD desk file -> Pick the SQL Server 2008 R2 ISO

For an CD/DVD’s click Devices -> CD/DVD Devices -> Host Drive 

![][2]

 [2]: /assets/img/old/SQL_Server_Device.png

Click Run Setup when prompted.

If you get prompted to enable the .Net Framework Core role, click OK.

SQL Server Installation Center should come up. Click Installation, and then New Installation or add features to an existing installation.

![][3]

 [3]: /assets/img/old/SQL_Server_Installation_Center.png

Everything should pass on the Support Setup, Click OK.

![][4]

 [4]: /assets/img/old/SQL_Server_Support_Rules.png

Enter your product key and hit next

![][5]

 [5]: /assets/img/old/SQL_Server_Key.png

Accept the Terms and Conditions and click Next.

Click Install which will install the SQL Support Files.

Now for the actual Setup.

All the tests should pass, click next.

![][6]

 [6]: /assets/img/old/SQL_Server_More_Support_Rules.png

Click SQL Server Feature Installation , click Next

![][7]

 [7]: /assets/img/old/SQL_Server_Setup_Role.png

Click Select All, Click Next

![][8]

 [8]: /assets/img/old/SQL_Server_Feature_Selection.png

Everything should pass on the next step, click next.  
![][9]

 [9]: /assets/img/old/SQL_Server_Installation_Rules.png

Make sure Default instance is selected, click next

![][10]

 [10]: /assets/img/old/SQL_Server_Instance_Configuration.png

Disk Space requirements should pass (as we have 50 gigs), click Next

Next is assigning the accounts to the services. Since this is just a dev box I use the system account, I’m not worried about security. Click Use the same account for all SQL Server services, and then choose NT AUTHORITYSYSTEM. Click Next.

For Setting up the Database Engine Account Provisioning, click “Add Current User” (which will add the administrator account you’re logged in as). Click Next

![][11]

 [11]: /assets/img/old/SQL_Server_Database_Engine_Config.png

For Setting up the Analysis Service Account Provisioning, click “Add Current User” (which will add the administrator account you’re logged in as). Click Next

![][12]

 [12]: /assets/img/old/SQL_Server_Analysis_Services.png

Make sure “Install the native mode default configuration” is selected, and click Next.

![][13]

 [13]: /assets/img/old/SQL_Server_Reporting_Services_Config.png

Error Reporting, click Next

Installation Configuration Rules should pass, click Next

Ready to Install screen, click Next

Kick back and have a beer as this take a while to install

Success!

![][14]

 [14]: /assets/img/old/SQL_Server_Config.png

I would suggest a quick reboot of the virtual machine at this point. Once the machine has rebooted, lets take a snapshot up to where we are (in case something goes bad in the future, we can go back to this snapshot).

Click Machine -> Take Snapshot

![][15]

 [15]: /assets/img/old/SQL_Server_Snapshot.png

Give it a quick name, and then hit save.

![][16]

 [16]: /assets/img/old/SQL_Server_Snapshot_Name.png

Let’s go create our [domain controller][17].

 [17]: http://www.ryanonrails.com/2011/02/21/creating-a-domain-controller/