---
title: Installing / Configuring SharePoint 2010
categories: [Microsoft]
---


Let’s load up the Share Point 2010 ISO. Click Devices -> CD/DVD Devices -> Choose a virtual CD/DVD disk file -> Pick the Share Point 2010 64 bit ISO. Then choose “Install software prerequisites”

![][2]

 [2]: /assets/img/old/CRM2011_Sharepoint2010_Choose_V_Disk_1.png

![][3]

 [3]: /assets/img/old/CRM2011_Sharepoint2010_Pre_Req_2.png

Click Next on the Welcome Screen

Accept the Terms and Conditions, Click Next

Sometimes (it seems random) but when adding the prerequisites it fails out. Don’t fear! Just manually add the item that it failed on. Just google whatever item failed (Microsoft Server Speech Recognition Language – TELE(en-US) successfully for example) find the installation file and then re-run the prerequisites. I’ve had it happen randomly with 2-4 different modules.

Click Finish once the installation is complete.

![][4]

 [4]: /assets/img/old/CRM2011_Sharepoint2010_Install_Complete_3.png

Click Install SharePoint Server.

Punch in your SharePoint key, Click Next.

Accept the Software License Terms, Click Next.

Leave the default installation locations. Click Install Now.

![][5]

 [5]: /assets/img/old/CRM2011_Sharepoint2010_File_Loc_4.png

Once it finishes, make sure the “Run the SharePoint Products Configuration Wizard now” is checked and click Close.

Click Next on the welcome screen.

Click Yes to the start/restart services prompt.

![][6]

 [6]: /assets/img/old/CRM2011_Sharepoint2010_Serv_Restart_5.png

Make sure “Create new server farm” is selected, Click Next.

![][7]

 [7]: /assets/img/old/CRM2011_Sharepoint2010_Connect_Farm_6.png

Punch in a Database server name. If you’ve been following my other tutorials you’ll want to use Administrator/pass@word1 for the Database Access Account. Click Next.

![][8]

 [8]: /assets/img/old/CRM2011_Sharepoint2010_Database_Settings_7.png

If you get the error “Cannot connect to database master at SQL Server at . The database might not exist, or the current user does not have permission to connect to it.” Make sure to double check your Database Server name. Using “.” (localhost) should work if you’ve been following the previous guides. If that checks out, make sure your Administrator Account has securityadmin, dbcreator and sysadmin roles in SQL Server 2008 R2.

On the Specify Farm Security Settings page, punch in “pass@word1” for the passphrase. Click next.

![][9]

 [9]: /assets/img/old/CRM2011_Sharepoint2010_Security_Settings_8.png

Configure SharePoint Central Administration Web Application page, click Next.

![][10]

 [10]: /assets/img/old/CRM2011_Sharepoint2010_Config_Cent_Admin_9.png

Completing the SharePoint Products Configuration Wizard screen, Click Next.

![][11]

 [11]: /assets/img/old/CRM2011_Sharepoint2010_Comp_Wizard_101.png

On the Configuration Successful screen, click Finish.

I would take a snapshot at this point. Named “After Share Point installation”.

The Farm configuration wizard should open up, click “Start the Wizard”.

![][12]

 [12]: /assets/img/old/CRM2011_Sharepoint2010_Config_Farm_1_11.png

Restart the machine at this point. I kept getting error “The service applications for the service “Search Service Application” could not be provisioned” on the next step until I restarted.

Click Next on the Service Account step.

![][13]

 [13]: /assets/img/old/CRM2011_Sharepoint2010_Config_Farm_2_12.png

Punch in a Title name for the CRM Server. In the URL point it to Sites, and then punch in the name you gave your SharePoint server. Click OK.

![][14]

 [14]: /assets/img/old/CRM2011_Sharepoint2010_Config_Farm_3_13.png

The Farm Configuration is complete now! 

Click Finish. Now you can go to http://sites/CRMSharepointTest and you will see your SharePoint server.

![][15]

 [15]: /assets/img/old/CRM2011_Sharepoint2010_Config_Farm_4_14.png

Let’s go and [change the CRM 2011 port][16].

 [16]: http://www.ryanonrails.com/2011/02/25/changing-the-crm-server-port/