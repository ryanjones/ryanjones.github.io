---
title: 'Using Sharepoint 2010 SOAP/Web Client - C#'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


I’ve been meaning to post this for a while since I had some annoying issues with CAML and Sharepoint 2010. Here’s the structure I’m using:

![][2]

 [2]: /assets/img/old/sp2010_structure.bmp

I’ve had to do quite a few things with the Sharepoint 2010 web services and web client object. I figured I should post some solutions as quite a few of these are undocumented. My goal was not to have to import the SP Library and just use the web services along with the basic GET PUT methods of the WebClient.

Create a Folder with SharePoint 2010 via SOAP Web Services – UpdateListItems  
[http://www.ryanonrails.com/2011/10/11/create-a-folder-with-sharepoint-2010-via-soap-web-services-updatelistitems/][100]

 [100]: http://www.ryanonrails.com/2011/10/11/create-a-folder-with-sharepoint-2010-via-soap-web-services-updatelistitems/ "http://www.ryanonrails.com/2011/10/11/create-a-folder-with-sharepoint-2010-via-soap-web-services-updatelistitems/"

Upload a document to Sharepoint 2010 via WebClient  
[http://www.ryanonrails.com/2011/09/17/upload-a-document-to-sharepoint-2010-via-webclient/][3]

 [3]: http://www.ryanonrails.com/2011/09/17/upload-a-document-to-sharepoint-2010-via-webclient/ "http://www.ryanonrails.com/2011/09/17/upload-a-document-to-sharepoint-2010-via-webclient/"

Retrieve document ID from SharePoint 2010 via SOAP Web Services – GetListItems  
[http://www.ryanonrails.com/2011/08/30/retrieve-document-id-from-sharepoint-2010-via-soap-web-services-getlistitems/][4]

 [4]: http://www.ryanonrails.com/2011/08/30/retrieve-document-id-from-sharepoint-2010-via-soap-web-services-getlistitems/ "http://www.ryanonrails.com/2011/08/30/retrieve-document-id-from-sharepoint-2010-via-soap-web-services-getlistitems/"

Update Custom Columns from SharePoint 2010 via SOAP Web Services – UpdateListItems  
[http://www.ryanonrails.com/2011/08/30/update-custom-columns-from-sharepoint-2010-via-soap-web-services-updatelistitems/][5]

 [5]: http://www.ryanonrails.com/2011/08/30/update-custom-columns-from-sharepoint-2010-via-soap-web-services-updatelistitems/ "http://www.ryanonrails.com/2011/08/30/update-custom-columns-from-sharepoint-2010-via-soap-web-services-updatelistitems/"

Check in a file into SharePoint 2010 via SOAP Web Services – CheckInFile  
[http://www.ryanonrails.com/2011/08/30/check-in-a-file-into-sharepoint-2010-via-soap-web-services-–-checkinfile/][6]

 [6]: http://www.ryanonrails.com/2011/08/30/check-in-a-file-into-sharepoint-2010-via-soap-web-services-–-checkinfile/ "http://www.ryanonrails.com/2011/08/30/check-in-a-file-into-sharepoint-2010-via-soap-web-services-–-checkinfile/"

Delete document from SharePoint 2010 via SOAP Web Services – UpdateListItems  
[http://www.ryanonrails.com/2011/08/30/delete-document-from-sharepoint-2010-via-soap-web-services-updatelistitems/][7]

 [7]: http://www.ryanonrails.com/2011/08/30/delete-document-from-sharepoint-2010-via-soap-web-services-updatelistitems/ "http://www.ryanonrails.com/2011/08/30/delete-document-from-sharepoint-2010-via-soap-web-services-updatelistitems/"

Download a document from Sharepoint 2010 via WebClient  
[http://www.ryanonrails.com/2011/10/11/download-a-document-from-sharepoint-2010-via-webclient/][8]

 [8]: http://www.ryanonrails.com/2011/10/11/download-a-document-from-sharepoint-2010-via-webclient/ "http://www.ryanonrails.com/2011/10/11/download-a-document-from-sharepoint-2010-via-webclient/"