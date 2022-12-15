---
title: Hooking CRM 2011 and Share Point 2010 together
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


CRM2011 – SharePoint 2010 Integration? Glue CRM 2011 & Share Point 2010 together? Make CRM 2011 and Share Point 2010 converse? I wasn’t sure what to call this exactly. “Hooking together” works for me!

Now that we have a CRM 2011 instance and a Share Point site working, let’s get them connected up! Go to this website and download Microsoft Dynamics CRM 2011 List Component for Microsoft SharePoint Server 2010:



Accept the License Terms.

Extract the files to a folder (I chose C:\CRM List).

You will get a prompt “The Installation is complete.” Click OK.

Let’s go over to the Share Point Central Administration Server to install the list component we just extracted. Connect to http://localhost:48835/ (your port might be different, be aware of this). Click Manage web applications.

![][2]

 [2]: /assets/img/old/CRM_Sharepoint_Glue_Manage_Web_App_1.png

Click the new Share Point site, and then “General Settings” (the blue cogs).

![][3] 

 [3]: /assets/img/old/CRM_Sharepoint_Glue_Manage_General_Settings_2.png

Scroll down to Browser File Handling and choose Permissive, Click OK.

![][4]

 [4]: /assets/img/old/CRM_Sharepoint_Glue_Permissive_3.png

Let’s head back over to our new Share Point Site. Click Site Actions up top left, and then “Site Settings”.

![][5]

 [5]: /assets/img/old/CRM_Sharepoint_Glue_Site_Settings_4.png

Under Galleries click “Solutions”.

![][6]  
Click the Word “Solutions” up top (you have to click the word “Solutions”, even though it looks selected), and then click “Upload Solution”.

 [6]: /assets/img/old/CRM_Sharepoint_Glue_Solutions_5.png

![][7] 

 [7]: /assets/img/old/CRM_Sharepoint_Glue_Upload_Solution_6.png

Select the .wsp component that we extracted wayyy back at the top of this. I used C:CRM List as my extract folder. Click OK.

![][8]

 [8]: /assets/img/old/CRM_Sharepoint_Glue_Pick_Component_7.png

You’ll get prompted at this point, I couldn’t active the control on this screen (but it still needs to be done). We need to make sure some services are running to activate the solution. Click Close.

![][9]

 [9]: /assets/img/old/CRM_Sharepoint_Glue_Activate_Solution_8.png

Head back to the Share Point Central Administration. http://localhost:48835.  
Found at 

Click System Settings -> Manage Services on this server

![][10]

 [10]: /assets/img/old/CRM_Sharepoint_Glue_Manage_Services_9.png

Click Start beside “Share Point Foundation Sandboxed Code Service”. I also started “Microsoft SharePoint Foundation Subscription Settings Service (by accident)” so that’s why that ones started.

![][11]

 [11]: /assets/img/old/CRM_Sharepoint_Glue_Start_Service_10.png

Now to head back to our Share Point site http://localhost:39083/

![][12]

 [12]: /assets/img/old/CRM_Sharepoint_Glue_Site_Settings_11.png

Under Galleries click “Solutions”.

![][13]

 [13]: /assets/img/old/CRM_Sharepoint_Glue_Solutions_12.png

Click Solutions again, select crmlistcomponent, and the click “Activate” up top. Activate is now un-greyed out! Click Activate!

![][14]

 [14]: /assets/img/old/CRM_Sharepoint_Glue_Real_Activation_13.png

The solution has now been activated! Hurray!

![][15]

 [15]: /assets/img/old/CRM_Sharepoint_Glue_View_Activated_14.png

There seems to be some confusion whether or not you need to run a power shell script to enable Activation of Share Point 2010 solutions (AllowHtcExtn). According to what I’ve read, you would need to run this if Share Point 2010 is running on a domain controller. I didn’t have to do this (and we’re on a domain controller), and I’ve yet to run into a problem with .htc stuff. Even in the Microsoft Dynamics CRM 2011 Readme  it says:  
“If you are using Microsoft SharePoint Server 2010 (On-Premises), you must add .htc extensions to the list of allowed file types:  
a. Copy the AllowHtcExtn.ps1 script file to the server that is running Microsoft SharePoint Server 2010.  
b. In the Windows PowerShell window or in the SharePoint Management Console, run the command: AllowHtcExtn.ps1 .  
Example: AllowHtcExtn.ps1 http://servername”

Some people say the script works for them , and some say that using just the blog method (what we did) works   
The sharepoint configuration is complete at this point. You probably want to take a snapshot, name it “After Sharepoint Configuration”. Let’s head over to our CRM server (localhost:85).

In CRM Click Settings -> Document Management -> Document Management Settings

![][16]

 [16]: /assets/img/old/CRM_Sharepoint_Glue_Doc_Man_Settings_15.png

Select the entities that you want to have documents enabled on. This will create a “Documents” area when you open an instance of the entity. I’ll just leave the defaults for now. At the bottom punch in your Share Point site that you’ve created and click Next. This is the Share Point server we installed the list component on. You’re not allowed to use localhost:port, just use the computer name:port like below.

![][17]

 [17]: /assets/img/old/CRM_Sharepoint_Glue_Doc_Man_Settings_Entity_16.png

Don’t select the box, otherwise it will relate the files to those entities. Without checking the box you will end up with something like Site/EntityName/Record Name (which is what I want, especially if you’re using custom entities). Click Next.

![][18]

 [18]: /assets/img/old/CRM_Sharepoint_Glue_Doc_Man_Settings_Folder_17.png

If “Libraries are being created in the path”, click Next.

![][19]

 [19]: /assets/img/old/CRM_Sharepoint_Glue_Doc_Man_Settings_Warn_18.png

Everything should “Succeed”, Click Finish.

![][20]

 [20]: /assets/img/old/CRM_Sharepoint_Glue_Doc_Man_Settings_Succeeded_19.png

Let’s test this bad boy out now. 

Create a new account called “Test”.

![][21]

 [21]: /assets/img/old/CRM_Sharepoint_Glue_Test_Record_20.png

Click Save! Click “Documents” on the left side. You’ll get a prompt saying that the folder (Test) is being created under “Account”. Click OK.

![][22]

 [22]: /assets/img/old/CRM_Sharepoint_Glue_Folder_Loc_21.png

Click Add.

![][23]

 [23]: /assets/img/old/CRM_Sharepoint_Glue_Add_Doc_22.png

Now you’ll probably get these errors! /crmgrid/scripts/DialogContainer.js and 403 FORBIDDEN! Depressing. The only real information on this error was here:  . It wasn’t very clear, but I stumbled through it. It seems that CRM 2011 doesn’t enjoy being called localhost. Let’s fix these up.

![][24]

 [24]: /assets/img/old/CRM_Sharepoint_Glue_Forbidden_24.png

![][25]

 [25]: /assets/img/old/CRM_Sharepoint_Glue_DialogContainer_23.png

The fix for this was to run inetmgr -> Click Microsoft Dynamics CRM -> click Stop

![][26]

 [26]: /assets/img/old/CRM_Sharepoint_Glue_Stop_CRM_25.png

Click “Bindings…” on the right side. Click “Edit” on the items that show “localhost” and change it to my machine name: “win-b80icqrvluf”. This is so it has a a “real” name to connect to.

Before:

![][27]

 [27]: /assets/img/old/CRM_Sharepoint_Glue_Binding_Change_1_26.png

After:

![][28]

 [28]: /assets/img/old/CRM_Sharepoint_Glue_Binding_Change_2_27.png

Now click “Start” on the right side.

![][29]

 [29]: /assets/img/old/CRM_Sharepoint_Glue_Start_CRM_28.png

Head back over to the CRM (http://win-b80icqrvluf:85/CRMTest/main.aspx) make sure to use the host name, as it might give you the error if you use localhost. Open your Test Account again.

![][30]

 [30]: /assets/img/old/CRM_Sharepoint_Glue_View_Test_29.png

Click Documents -> Add, you should now see this popup (it can take a while to load for the first time on the VM). If you continue to get the error, stop both CRM 2011 and Share Point 2010 servers and restart them. If that doesn’t work, try restarting the whole server.

![][31]

 [31]: /assets/img/old/CRM_Sharepoint_Glue_Upload_30.png

Pick a file, and click OK.

![][32]

 [32]: /assets/img/old/CRM_Sharepoint_Glue_Upload_File_31.png

The file should be uploaded to Share Point now.

![][33]

 [33]: /assets/img/old/CRM_Sharepoint_Glue_View_In_CRM_32.png

Head over to Share Point at http://win-b80icqrvluf:39083 and click “All Site Content” or “Libraries”.

![][34]

 [34]: /assets/img/old/CRM_Sharepoint_Glue_All_Site_Content_33.png

Click Account.

![][35]

 [35]: /assets/img/old/CRM_Sharepoint_Glue_Open_Account_34.png

You can see that CRM has created a folder “Test” (for our record). It creates 1 folder per record. Click it to see the files associated to that record!!

![][36]

 [36]: /assets/img/old/CRM_Sharepoint_Glue_Open_Test_35.png

The files associated to the record “Test” in Accounts.

![][37]

 [37]: /assets/img/old/CRM_Sharepoint_Glue_View_In_SP_36.png

Share Point and CRM have combined into a super awesome force of doom. But we’re still missing 1 core piece of functionality (due to not picking a port when we installed CRM).

Let’s go [fix our CRM 2011 web services][38].

 [38]: http://www.ryanonrails.com/2011/02/25/changing-crm-2011-web-services-port-via-deployment-manager/