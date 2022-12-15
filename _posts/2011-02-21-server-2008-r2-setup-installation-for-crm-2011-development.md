---
title: 'MS Server 2008 R2 Setup & Installation for CRM 2011 Development'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Before we start setting up Server 2008 R2, the default way to break out of the VM back into windows is “CTRL”

Fire up the virtual machine by clicking start.

At the first Run Wizard -> Click Next

Pick the installation media you want to use (I have an MSDN ISO, so I picked my MSDN ISO). You can use the actual Server 2008 R2 CD/DVD also if you’d like -> Click Next

![][2]

 [2]: /assets/img/old/Server_2008_Installation_Media.png

Click Finish on the First Run Wizard Summary.

![][3]

 [3]: /assets/img/old/Server_2008_Operating_Sys_Pick.png

Picked Windows Server 2008 R2 (Full Installation), then Next.

Accept the Terms and Conditions

Click Custom (advanced).

Click Next on Disk 0.

![][4]

 [4]: /assets/img/old/Server_2008_Operating_Sys_Pick1.png

Windows Server 2008 R2 will now install!

You will get prompted to change the admin password for the first time. I stick with the good ole “pass@word1”.

Server 2008 is installed and ready to rock.

Let’s do some quick maintenance. Click the Server Manager on the task bar (the picture is a toolbox server). Click “Configure IE ESC”.

![][5]

 [5]: /assets/img/old/Server_2008_Configure_IE_ESC.png

Click off for both Administrators and Users. This will stop you from getting blocked when trying to access the web.

![][6]

 [6]: /assets/img/old/Server_2008_Configure_IE_ESC_Off.png

Let’s get updated! Click Start -> Run Windows update. (I closed the other configuration screens at this point).  
![][7]

 [7]: /assets/img/old/Server_2008_Windows_Updates.png

Turn on Automatic updates (we’re not worried about security fixes as it’s just a dev box, but some of the fixes can fix compatibility between software).

Update the machine until no more updates are found.

Let’s head over to [ To get our guest additions installed][8].

 [8]: http://www.ryanonrails.com/2011/02/21/installing-guest-additions-for-an-oracle-virtual-box-machine/