---
title: 'Creating a Folder on Sharepoint 2010 via Web Service Lists.asmx  and UpdateListItems'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Quick explanation of how UpdateListItems (sharepoint web service) works when trying to add a folder to a document library.

I was using this code from [here][1] and I kept getting “Exception of type ‘Microsoft.SharePoint.SoapServer.SoapServerException’” thrown.

 [1]: http://blogs.msdn.com/b/crm/archive/2006/10/23/creating-folders-in-sharepoint-document-libraries.aspx

First off, it’s hella annoying that I don’t get any other error message than that. I wasted some time figuring out what I did wrong.

Here’s the code:
```csharp

Lists lists = new Lists();
lists.Url = "http://server/lists.asmx";
lists.Credentials = new NetworkCredential("username", "pass", "domain");

System.IO.StringWriter sw = new System.IO.StringWriter();
System.Xml.XmlTextWriter xw = new System.Xml.XmlTextWriter(sw);
xw.WriteStartDocument();

// Build batch node
xw.WriteStartElement("Batch");
xw.WriteAttributeString("OnError", "Return");
xw.WriteAttributeString("RootFolder", "");

// Build method node
xw.WriteStartElement("Method");
// Set transaction ID
xw.WriteAttributeString("ID", Guid.NewGuid().ToString("n"));
xw.WriteAttributeString("Cmd", "New");

// Build field ID
xw.WriteStartElement("Field");
xw.WriteAttributeString("Name", "ID");
xw.WriteString("New");
xw.WriteEndElement(); // Field end

// Build FSObjType
xw.WriteStartElement("Field");
xw.WriteAttributeString("Name", "FSObjType");
xw.WriteString("1");  // 1= folder
xw.WriteEndElement(); // Field end

// Build Base Name field from folder
xw.WriteStartElement("Field");
xw.WriteAttributeString("Name", "BaseName");
xw.WriteString("TestFolder");
xw.WriteEndElement(); // Field end

// Close Method & Batch elements
xw.WriteEndElement(); // Method end
xw.WriteEndElement(); // Batch end

xw.WriteEndDocument();

System.Xml.XmlDocument batchElement = new System.Xml.XmlDocument();
batchElement.LoadXml(sw.GetStringBuilder().ToString());

// Send update request to sharepoint to create the document folder
System.Xml.XmlNode result =
    lists.UpdateListItems("Document Library Display Name"
   ,batchElement);

return result.ToString();
```


What I did wrong was use the actual path of the document library when I should have used the display name (I was using documentlibraryname, which is the actual url path). It was throwing this exception because it couldn’t find the “listName”. I’m not sure if it would be possible, but some type of descriptive error coming back would be nice.

Now on to how the method actually works. When you call UpdateListItems a few things happen:
If the folder doesn’t exist, it creates it.
If the folder exists it returns an error in a string (which you can just ignore and continue on happily).

By using the display name it causes a ton of grief since when I’m writing CRM 2011 plugins I can do context.LogicalEntityName to get the entity name (which would return documentlibraryname).
