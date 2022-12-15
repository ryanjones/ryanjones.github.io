---
title: 'Delete document from SharePoint 2010 via SOAP Web Services  - UpdateListItems'
categories:
  - Microsoft / CRM / SharePoint / SSRS
---


Delete a document from SharePoint 2010. You NEED both the ID and the FileRef.
```csharp
// Delete the sharepoint document
System.Text.StringBuilder oSb = new System.Text.StringBuilder();
oSb.Append("     <Batch OnError=\"Continue\"> ");
oSb.Append("         <Method ID=\"1\" Cmd=\"Delete\"> ");
oSb.Append("             <Field Name=\"ID\">" + fileID + "</Field> ");
oSb.Append("             <Field Name=\"FileRef\">" + URLofFile + "</Field> ");
oSb.Append("        </Method>");
oSb.Append("    </Batch>");
 
string sResult = oSb.ToString();
XmlDocument CAMLqueryXML = new XmlDocument();
CAMLqueryXML.LoadXml(sResult);
 
// Execute GetListItems
XmlNode queryResults = lists.UpdateListItems(SPLibraryName, CAMLqueryXML);
```