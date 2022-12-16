---
title: Check in a file into SharePoint 2010 via SOAP Web Services – CheckInFile
categories: [Microsoft]
---


Checking in a a file into Sharepoint using Lists.asmx.
```csharp
// Check the file in to sharepoint
lists.CheckInFile(destinationUrl, "Text for the checkin", "1");
```
A string representation of the values 0, 1 or 2, where 0 = MinorCheckIn, 1 = MajorCheckIn, and 2 = OverwriteCheckIn.