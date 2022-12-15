---
title: Download a document from Sharepoint 2010 via WebClient
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Here’s how I use the WebClient object to download a pdf from Sharepoint 2010:
```csharp
string uri = "wwww.google.ca";
string filepath = "waffles.pdf";
 
WebClient wc = new WebClient();
wc.Credentials = loginCredentials;
byte[] generatedPdf = wc.DownloadData(uri + "/" + filePath);
```